---
title: "When do you need MX, SPF, DKIM, and DMARC"
date: 2022-12-30T18:55:22+08:00
---
Setting up email configuration for your domain usually involves MX, SPF, DKIM, and DMARC.

When do you exactly need them?

If you are only receiving email:

  - You only need MX, because it tells the senders where your mail receiving servers are.

If you are sending email:

  - You will need SPF, DKIM, DMARC, and (highly recommended) MX.

These are records that verify sender's authenticity.

But, why do you need MX if you are sending emails?

Sometimes the emails that you sent might get bounced, and you'll receiving notification about the event through email. Without MX record, the notification email will not know where to go.
