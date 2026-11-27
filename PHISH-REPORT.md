# Spot the Phish: Report

Use the evidence codes from the handout (H1–H3, A1–A2, L1–L4, P1–P6, R1–R3, G1–G4). In *Key evidence*, quote the exact header values, domains or URLs.

## Message 01: Legitimate

**Evidence codes:** G1 (dmarc), G2 (links), G3 (not sensitive), G4 (regular), H3 (different return path, explained)

This pay-statement notice comes from `From: "Kestrel Payroll" <noreply@kestrelpay.example>`. Kestrel sent it through a mail relay service, so the envelope sender is `Return-Path: <bounces+k7Q2x9@mailrelay.example>` and the Received chain runs from `app-03.kestrelpay.example` through `out-14.mailrelay.example`. At first glance that looks like H3, but DKIM settles it: `dkim=pass header.d=kestrelpay.example header.s=k1` and `dmarc=pass (p=REJECT) header.from=kestrelpay.example`. DMARC is aligned through DKIM, and kestrelpay.example is DevTalk's real payroll provider. The `Message-ID: <20261109110217.7731.statements@kestrelpay.example>` also points to Kestrel. The only link, `href="https://www.kestrelpay.example"`, matches its visible text. The message asks for nothing: it tells Jordan to "sign in to Kestrel the way you normally do" and states that "Kestrel will never ask for your password, MFA code or banking details by email."

**What Jordan should do:** To view the statement, Jordan can sign in to Kestrel the usual way, without using the email, and go to Pay › Statements.

## Message 02: Phishing

**Evidence codes:** H2 (different reply-to), H3 (bulk mailing, dkim=none, dmark=none), A1 (dkim, dmark), L1 (different link), L2 (combosquatting, subdomain), P1 (urgency), P2 (authority), P3 (fear), R1 (credentials)

The sender, `From: "DevTalk Support" <support@devtalk-security.example>`, uses a combosquatted domain, not devtalk.example. Replies would go to a free webmail account, `Reply-To: devtalk.helpdesk@freemail.example`. Both `Return-Path: <bounce-7731@bulk-mailer.example>` and `Message-ID: <a81f0c2e9b.7731@bulk-mailer.example>` point to a bulk mailing system unrelated to DevTalk. Authentication shows `dkim=none (message not signed)` and `dmarc=none (no policy published) header.from=devtalk-security.example`. The fallback link displays `https://devtalk.example/login`, but its real destination is `href="https://devtalk.example.account-verify.example/login?u=jordan.tremblay"`. This is the subdomain trick: the domain that actually controls the page is account-verify.example. The body claims authority ("DevTalk Trust & Safety Team"), creates urgency and fear ("verify your identity within 24 hours or your account and all of your posts will be permanently suspended"), and the "Verify my account" button leads to a login page that would capture Jordan's credentials.

**What Jordan should do:** Don't click or reply, and use Report Phish. If Jordan is worried about the account, they should type devtalk.example into the browser and sign in as usual.

## Message 03: Legitimate

**Evidence codes:** G1 (dmarc, correct domain), G2 (links), G3 (not sensitive), G4 (regular)

This maintenance notice comes from `From: "DevTalk IT Service Desk" <servicedesk@devtalk.example>`, with a matching `Return-Path: <servicedesk@devtalk.example>`. It was received `from mail-int-01.devtalk.example [192.0.2.25]`, DevTalk's own internal mail server. Every check passes for DevTalk's domain: `spf=pass smtp.mailfrom=devtalk.example`, `dkim=pass header.d=devtalk.example header.s=dt2026` and `dmarc=pass (p=NONE) header.from=devtalk.example`. The `Message-ID: <CHG-2026-1188.servicedesk@devtalk.example>` carries the change number cited in the body. The only link, `href="https://status.devtalk.example"`, matches its text. The message says "No action is needed from you," states that "the Service Desk will never ask for your password or MFA code," and can be verified independently: "Call ext. 4357 or open a ticket on the intranet."

**What Jordan should do:** No action is needed. Expect the VPN and admin console to be down on Saturday, November 14, from 10 p.m. to 2 a.m. If unsure, Jordan can confirm change CHG-2026-1188 on the intranet or at ext. 4357.

## Message 04: Phishing

**Evidence codes:** H1 (free webmail), A2 (free webmail), P1 (urgency), P2 (authority), P5 (secrecy), R2 (gift card), R3 (skip procedures)

The message appears to come from an executive, but the address is a free webmail account: `From: "Dana Okafor" <dana.okafor.devtalk@freemail.example>`. The `Return-Path: <dana.okafor.devtalk@freemail.example>` and `Message-ID: <CAF8k2p0Q1x+dO@mail.freemail.example>` confirm it came from the webmail provider, not DevTalk. Authentication passes everywhere (`spf=pass smtp.mailfrom=freemail.example`, `dkim=pass header.d=freemail.example`, `dmarc=pass (p=NONE) header.from=freemail.example`), but only for freemail.example, which proves nothing about Dana. This is a classic gift-card scam. It leans on authority ("at the board offsite") and urgency ("need a favour handled quietly before 6"), and asks Jordan to "buy 6 x $250 digital gift cards (Snowline Market works), scratch the backs and reply with the codes." It demands secrecy and blocks verification: "Keep this between us" and "I can't take calls, so just reply here." That skips every normal purchasing procedure.

**What Jordan should do:** Don't reply or buy anything. Contact Dana using the number in the staff directory, use Report Phish, and tell Finance and IT Security.

## Message 05: Phishing

**Evidence codes:** H2 (reply-to), H3 (not claimed sender), A1 (authentication fails), L4 (link shortener), P1 (urgency), P3 (fear), R2 (payment)

This courier notice claims to be from `From: "Snowline Courier" <notify@snowline-delivery.example>`, but replies would go to `Reply-To: claims.snowline@freemail.example`. The real origin is a rented virtual server: `Return-Path: <x3f9a@vps-2231.cheaphost.example>`, Received `from vps-2231.cheaphost.example (unknown [203.0.113.181])`, and `Message-ID: <0b9e1a7c44d2.SN48120377CA@vps-2231.cheaphost.example>`. Authentication fails or is missing: `spf=softfail (sender IP 203.0.113.181 is not authorized) smtp.mailfrom=cheaphost.example`, `dkim=none`, and `dmarc=none (no policy published) header.from=snowline-delivery.example`. The "Pay fee and schedule delivery" button points to `href="https://snip.example/Xk29pQ"`, a link shortener that hides the real destination. The message pressures Jordan with a deadline and a penalty ("Pay within 48 hours … Parcels not claimed will be returned to the sender") and asks for payment, "a customs and handling fee of $2.99 CAD." The small amount is bait to collect card details.

**What Jordan should do:** Don't click or pay, and use Report Phish.

## Message 06: Legitimate

**Evidence codes:** G1 (dmarc), G3 (regular), G4 (procedures), G2 (no links)

This password-change notice comes from `From: "DevTalk Account Security" <security@devtalk.example>`, with a matching `Return-Path: <security@devtalk.example>`. It was received `from mail-int-01.devtalk.example [192.0.2.25]`. All checks pass for DevTalk: `spf=pass smtp.mailfrom=devtalk.example`, `dkim=pass header.d=devtalk.example header.s=dt2026` and `dmarc=pass (p=NONE) header.from=devtalk.example`. The `Message-ID: <pwchange.20261113011437.u1042@devtalk.example>` was created by DevTalk's own system. The HTML contains no `href=` at all. Instead, it tells Jordan to "type devtalk.example yourself, sign in, and go to Account › Security › Sign out everywhere." It asks for nothing ("If this was you, you don't need to do anything"), states that "DevTalk will never ask for your password, MFA code or recovery codes," and points to a phone number Jordan can verify, ext. 4357.

**What Jordan should do:** If Jordan changed the password on November 12, nothing. If not, they should type devtalk.example into a new tab, sign in, use Sign out everywhere, and call the Service Desk at ext. 4357.

## Message 07: Phishing

**Evidence codes:** A2 (domain), L1 (link), L2 (character substitution), P1 (urgency), P2 (authority), P3 (fear), R1 (credentials), R2 (banking details), R3 (skip procedures)

The sender, `From: "Kestrel Payroll" <notifications@kestre1pay.example>`, uses a lookalike domain: the digit "1" replaces the letter "l" in kestrelpay.example. The same lookalike appears in `Reply-To: payroll-support@kestre1pay.example`, `Return-Path: <bounce@kestre1pay.example>`, Received `from mta1.kestre1pay.example [203.0.113.52]` and `Message-ID: <dd-verify.88213.DT-1042@kestre1pay.example>`. Authentication passes (`spf=pass smtp.mailfrom=kestre1pay.example`, `dkim=pass header.d=kestre1pay.example`, `dmarc=pass (p=REJECT) header.from=kestre1pay.example`), but only for the lookalike, not for Kestrel. The fallback link displays `https://portal.kestrelpay.example/direct-deposit`, but its real destination is `href="https://portal.kestre1pay.example/direct-deposit/verify?emp=DT-1042"`. The body sets a deadline ("by 5:00 p.m. on Friday, November 13"), threatens a penalty (pay "held until the next pay run"), and asks Jordan to "sign in and re-enter your banking information."

**What Jordan should do:** Don't click or enter anything. Use Report Phish and delete the message.

## Message 08: Phishing

**Evidence codes:** H1 (domain), H2 (reply-to), H3 (domain), A2 (lookalike), L1 (link), L3 (homograph), P4 (curiosity), P5 (secrecy)

The sender is `From: "Morgan Lee (via PaperLeaf)" <share@xn--rleaf-3ve9a0fb.example>`. The punycode domain decodes to `рареrleaf.example`, whose first four letters are Cyrillic (U+0440 U+0430 U+0440 U+0435). It is a homograph that looks like paperleaf.example. The display name claims a colleague, and the message borrows that colleague's real address for `Reply-To: "Morgan Lee" <morgan.lee@devtalk.example>`, a different domain from the From address. The `Return-Path: <share@xn--rleaf-3ve9a0fb.example>`, Received `from out.xn--rleaf-3ve9a0fb.example [203.0.113.9]` and `Message-ID: <share-7Fh2kQ.1794582768@xn--rleaf-3ve9a0fb.example>` all trace back to the homograph domain. Authentication passes (`dkim=pass header.d=xn--rleaf-3ve9a0fb.example`, `dmarc=pass (p=QUARANTINE) header.from=xn--rleaf-3ve9a0fb.example`), but only for that fake domain. The fallback link displays `https://docs.paperleaf.example/s/Q4-salary-bands`, but its real destination is `href="https://docs.xn--rleaf-3ve9a0fb.example/s/Q4-salary-bands?share=7Fh2kQ"`. The bait plays on curiosity ("Q4 Salary Bands - CONFIDENTIAL.xlsx") and secrecy ("Please don't forward").

**What Jordan should do:** Don't open the link and use Report Phish.

## Message 09: Legitimate

**Evidence codes:** G1 (dmarc), G3 (regular), G4 (regular procedures),  G2 (no links)

This security alert comes from `From: "DevTalk IT Security" <security@devtalk.example>`, with a matching `Return-Path: <security@devtalk.example>`, and was received `from mail-int-01.devtalk.example [192.0.2.25]`. All checks pass for DevTalk: `spf=pass smtp.mailfrom=devtalk.example`, `dkim=pass header.d=devtalk.example header.s=dt2026` and `dmarc=pass (p=NONE) header.from=devtalk.example`. The `Message-ID: <alert-2026-11-12.security@devtalk.example>` is DevTalk's own. The HTML contains no links. The advice matches known procedures: "Kestrel never asks for banking details by email. Direct deposit changes are made only through HR," "Genuine Kestrel emails come from kestrelpay.example," and "Reach Kestrel through the link on the intranet's HR page." It directs staff to "use the Report Phish button" and to "call the Service Desk at ext. 4357."

**What Jordan should do:** Report message 07 with Report Phish and delete it. If Jordan entered any details, they should call ext. 4357 right away.

## Message 10: Phishing

**Evidence codes:** H3 (not claimed sender), A1 (dmarc, dkim), L4 (QR code), P1 (urgency), P2 (authority), P3 (fear), R1 (credentials), R3 (skip procedures)

The message spoofs DevTalk's own Service Desk: `From: "DevTalk IT Service Desk" <servicedesk@devtalk.example>`. It did not come from DevTalk's mail-int-01 server. The headers show `Return-Path: <mailer@notice-hub.example>`, Received `from relay.notice-hub.example [203.0.113.140]` and `Message-ID: <7d1e0f3a.notice@relay.notice-hub.example>`. SPF passes only for the unrelated sender (`spf=pass smtp.mailfrom=notice-hub.example`), the message is unsigned (`dkim=none (message not signed)`), and DMARC fails for the domain it claims: `dmarc=fail (p=NONE dis=NONE) header.from=devtalk.example`. Instead of a link, it uses a QR code, `<img src="cid:mfa-qr@devtalk">` (`enroll.png`), which hides the destination. It pressures Jordan with authority and a deadline ("re-enroll multi-factor authentication (MFA) … by 5:00 p.m. today") and a penalty ("Accounts that are not re-enrolled will be locked"). It also asks for credentials directly: "sign in with your DevTalk username and password, and enter your current MFA code." It piggybacks on the real maintenance announced in message 03, which said "No action is needed from you" and that the Service Desk "will never ask for your password or MFA code."

**What Jordan should do:** Don't scan the QR code or enter credentials, and use Report Phish. To check whether any MFA change is real, Jordan should call the Service Desk at ext. 4357 or check the intranet.
