# Other customer data options

Besides native integrations, several methods are available to send Review Collect your daily list of customers to invite.

## Google Sheets (recommended)

Review Collect provides a standardized file. Each day, you add the new customers to invite. This method is simple and widely used by clients.

It can also be automated with tools like Zapier, Make or n8n.

## FTP/SFTP server

You drop a CSV file every day on an FTP server. Share the access credentials (login and password, or an authentication key) with your account manager, who automates the data retrieval.

## Custom API integration

If you use an internal tool such as a CRM or an ERP, a custom integration can automate the data flow. The implementation time depends on the technical complexity of your system.

## Expected data structure

| Field | Description |
|---|---|
| ID | Unique identifier (order or customer). Required for WhatsApp messages. |
| First name | Customer's first name. |
| Last name | Customer's last name. |
| Email | Email address. |
| Phone | International E.164 format, for example `+33612345678`. |
| Country | 2-letter country code, for example `FR`, `ES`, `DE`. |
| Language | 2-letter language code, for example `fr`, `en`, `de`. Optional, useful if you operate in several languages. |
| Purchase or signup date | Required for WhatsApp messages. |

## Related

- [Getting started](../getting-started.md)
- [Collect reviews](../collect.md)
