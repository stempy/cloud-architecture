# How to handle Email at scale

Emails a simple thing, everyone uses it. Often in applications we just send an email as we need it directly via the SMTP servers which is great for simple scenarios where we only have a few emails, however there can sometimes be delays with contacting smtp servers and the like, so when you have an api for instance that sends an email after an operation is completed, it can delay the return result of the request, in effect making the user wait...... providing a slow user experience.

One way to improve this experience is to use Azure to offload the email sending to another cloud hosted service, or more specifically using Azure Queues with an Azure Function (or WebJob) to handle actual emailing, this way the api just adds an email to a queue, and they get processed and scaled as needed.

For our email needs, emails would need to be sent on many kinds of activity, financial transactions, customer updates, administrative changes, long running reports containing PDF's or CSV's, or links to binaries. Frequent email's across many services. We also needed to track emails sent to everywhere... having a central hosted email service served this purpose superbly.

> NOTE: Remember to use Azure's `NameValidator` to validate container, queue, and blob names etc to ensure correct error trapping is available.


The general process would be like so:

**Client App**

1. Composition of Email in HTML, and any attachments if required.
2. If there were any attachments, they were sent up to an azure blob(s) container specifically for email attachments, and appropriate SAS tokens created.
3. An Azure Queue message was created with metadata such as attachment url(s) from azure blob storage and the HTML email body. We would also need the environment, and other metadata.

```json
{
    "from"   : "no-reply@ourcompany.com",
    "to"     : "email1@someone.com;email2@someone.com;...",
    "subject": "email subject",
    "body"   : "actual email body content in HTML|PLAIN",
    "source" : "source of email, ie specific api name, service etc",
    "env"    : "dev",
    "attachmentUrls" : [
        "https://blob1.pdf",
        "https://blob2.jpg"
    ],
    "data"   : {},
    "createdUtc" : "jsondate"
}
```

**Hosted Email Service**

The Email Service itself was hosted as an Azure Function (lightweight azure services for basic cloud hosted functionality).
It would pick up the email messages off the queue, deserialize to a object, download any attachments from azure blob storage and compose the actual email. Log an email transaction, and than send the email itself via an SMTP service.

**A simple email contract interface**

A contract for .NET.... This is based on ASP.NET Core's `IEmailSender`, without the dependencies.

```cs
    public interface ISimpleEmailSender
    {
        Task SendEmailAsync(string to, string subject, string body);
        Task SendEmailAsync(string to, string subject, string body, object data);

        Task SendEmailAsync(string to, string subject, string body, IEnumerable<string> attachmentUrls, object data);
        Task SendEmailAsync(string to, string subject, string body, IEnumerable<Attachment> attachments);
        Task SendEmailAsync(string to, string subject, string body, IEnumerable<Attachment> attachments, object data);
    }
```

** Alternatives **
This is by no means the only way to do it, we could easily store the emails and attachments in a datastore of some sort such as mongodb, sql, redis-cache. And in some ways this is nicer as attachments are directly encapsulated with the email metadata as one package, and than we could use Azure Queue to just provide an `id` to the data-record, and keep email for long-term storage, an archive, the only issue with this is storing binaries in a database can quickly explode the size of it. Again azure blobs come in handy for long-term binary storage of the attachments, with meta data stored in a quick lookup type table of some sort. The key point is separating actual sending of emails from the applications.

