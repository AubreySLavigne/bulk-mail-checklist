# Bulk Mail Authentication Checklist

Sending bulk emails requires a certain level of authentication to guarantee the best chance for deliverability.

This file will provide a framework to standardize which steps need to be taken. 

## Citation

URL: https://support.google.com/mail/?p=IPv6AuthError

## Notes

[ ] Authentication 
    [ ] Minimums
        [ ] Consisent Origin IP Address
        [ ] Valid Reverse IP Address Record, pointing to origin IP Address
        [ ] Same Address in the "From: " header
    [ ] Recommendations
        [ ] Sign Messages with DKIM, at least 1024 bits
        [ ] SPF Record
        [ ] DMARC Policy
    [ ] IPv6 Requirements
        [ ] PTR Record
        [ ] Pass one of the following [Testing Tool](https://mxtoolbox.com/SuperTool.aspx):
            [ ] SPF Check
            [ ] DKIM Check
    [ ] Subscription Requirements
        [ ] Opt-In
            [ ] Use of the following methods:
                [ ] Email asking to subscribe
                [ ] Manually Checking an opt-in box (automatically checking this box is not acceptable)
        [ ] Opt-Out - Must be allowed to do so
            [ ] Use of the following methods:
                [ ] Link in the body of emails to unsubscribe 
                    * Notes: 
                        * Nothing else, other than a confirmation message is necessary.
                        * Recommend sending the email with a `List-Unsubscribe` header (See Below)
                [ ] Replying to the email with an unsubscribe request
            [ ] Also recommend the following:
                [ ] Unsubscribe if a user automatically bounces multiple times
                [ ] Periodically send confirmation messages
                [ ] If there are more than one mailing list, allow the user to subscribe to each of them separately
                [ ] Include the email address (in the case the email was forwarded to another address)
                [ ] Support URL method for unsubscribing
            [ ] For 3rd party senders (if you are running a service for that allows users to send emails)
                [ ] Provide an email for people to report abuse (abuse@domain.com)
                [ ] Maintain up to date contact info in your WHOIS record
                [ ] Maintain up to date contact info on abuse.net
                [ ] Terminate (in a timely fashion) all users sending spam information
    [ ] Format Requirements
        [ ] Must: 
            [ ] Meet [RFC 5322](https://tools.ietf.org/html/rfc5322) and [HTML Standards](https://www.whatwg.org/html)
            [ ] Have a valid `Message-ID` header
        [ ] Should:
            [ ] Have a valid `Precedence: bulk` header
            [ ] Have a subject relevant to the body
        [ ] Should Not:
            [ ] attempt to hide the true sender
            [ ] Have the following violate the [Unicode Security Profile](http://www.unicode.org/reports/tr39/#Restriction_Level_Detection) - 'Highly Restrictive' level:
                * Authenticating Domain
                * Envelope and Payload From Domain
                * Reply-To Domain
                * Sender Domain
[ ] Email Categorization -- Helps with putting emails into correct category, not necessary for authorization
    [ ] Send Emails for different purposes 
        * From Separate Email Addresses within a domain
        * Form Separate domains or IP Addresses
    [ ] Keep each email containing only one type of information (promotional, updates, etc.)

## List-Unsubscribe Sample Headers

### One-Click

**Add This header to the requesting email**

```
List-Unsubscribe-Post: List-Unsubscribe=One-Click
List-Unsubscribe: <https://example.com/unsubscribe/opaquepart>
```

**An HTTP request being sent to the `https://example.com/unsubscribe/opaquepart` address defined above**

```
POST /unsubscribe/opaquepart HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 26

List-Unsubscribe=One-Click
```

### mailto

**Add a header to the requesting email (one of the following)**

```
List-Unsubscribe: <mailto:list@host.com?subject=unsubscribe>
List-Unsubscribe: (Use this command to get off the list)
 <mailto:list-manager@host.com?body=unsubscribe%20list>
List-Unsubscribe: <mailto:list-off@host.com>
```

## Ownership

I make no claim of ownership for the information contained in this file
