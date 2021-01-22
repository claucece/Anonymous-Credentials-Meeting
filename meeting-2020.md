# Anonymous Credentials Meeting 2020 Resources

Hello!

Thank you so much for having attended the Anonymous Credentials Meeting 2020 or
for being interested by it! Here you can find some resources of it.

## Slides

* For 'Privacy Pass at the IETF' by Ben Schwartz, the
  slides can be found [here](https://github.com/claucece/Anonymous-Credentials-Meeting/blob/main/slides/Privacy%20Pass%20RWC%202021%20(1).pdf).
* For 'Anonymous Credentials: hCaptcha' by Eli-Shaoul Khedouri, the
  slides can be found [here](https://github.com/claucece/Anonymous-Credentials-Meeting/blob/main/slides/hCaptcha%20-%20Anonymous%20Credentials%20Presentation%20RWC%202021%20(1).pdf).
* For 'Anonymous Credentials and Brave' by Goçalo Pestana and Iñigo Querejeta,
  the slides can be found [here](https://docs.google.com/presentation/d/1eB4A7vOCo-UrdQ85roo-IqfFWJZY_rvoIUt9hFuWvpc/edit#slide=id.p)
* For 'Trust Token API' by Steven Valdez,
  the slides can be found [here](https://docs.google.com/presentation/d/15BFZ4dNWBtOhjaR2RYNeVqke-xIdwtE63DUBpL-ry5Y/edit#slide=id.p)

## Additional resources

### Tor case

* [State of the Onion: Tokens](https://www.youtube.com/watch?v=542d7FooV84) by George Kadianakis
* [How to stop the onion denial (of service)](https://blog.torproject.org/stop-the-onion-denial) by George Kadianakis

### Facebook case

* [PrivateStats: De-Identified Authenticated Logging at Scale](https://research.fb.com/wp-content/uploads/2021/01/PrivateStats-De-Identified-Authenticated-Logging-at-Scale_final.pdf) by Sharon Huang, Subodh Iyengar, Sundar Jeyaraman, Shiv Kushwah, Chen-Kuei Lee, Zutian Luo, Payman Mohassel, Ananth Raghunathan, Shaahid Shaikh, Yen-Chieh and Sung Albert Zhang

### Brave case

* [Security and privacy model for ad confirmations](https://github.com/brave/brave-browser/wiki/Security-and-privacy-model-for-ad-confirmations)

### Links shared on the chat

* [Anonymity at RWC2021](https://www.youtube.com/watch?v=4eKwlSqGUi4)
* [Anonymous Tokens with Private Metadata Bit](https://eprint.iacr.org/2020/072.pdf)
  with its [implementation](https://github.com/mmaker/anonymous-tokens), and
  another [implementation in BoringSSL](https://boringssl.googlesource.com/boringssl/+/refs/heads/master/include/openssl/trust_token.h).
* Protocol for anonymous tokens used in the Norwegian COVID19 app, can be found
  [here](https://github.com/tjesi/anonymous-tokens)
* [An Accumulator Based on Bilinear Maps and Efficient Revocation for Anonymous Credentials](https://eprint.iacr.org/2008/539.pdf)
* [Compact Privacy Protocols from Post-quantum and Timed Classical Assumptions](https://link.springer.com/chapter/10.1007%2F978-3-030-44223-1_13)

### Notes from the meeting

### Introduction by Sofía Celi

As we know, there is a Privacy Pass Working Group (WG) is an effort of IETF. But
there are many use cases beyond it that we will like to tackle as well. Let’s
hear about them, and take them back to the WG.

### Ben Schwartz (IETF chair for Privacy Pass WG): Privacy Pass, current status

Tor users began seeing an increase in the number of CAPTCHAs. Cloudflare was
concerned about rate limiting.

Chrome initiative with Trust Token (late 2019). Steven Valdez is leading this
work. The W3C adopted this approach, and Chrome deployed this as part of its
origin experiment. There are few variants that have been developed by academics.

Finally, a new WG was formed at IETF: drafts around the protocol itself,
architecture and https.

We would like to know what is needed for them to be implemented, and help us
solve some open questions: how to model client linkability, tradeoffs between
metadata, anonymity set size, issuer consolidation pressure, and public key
commitment approaches.

### Eli-Shaoul Khedouri (hCaptcha): Overview of how it is used

* General verification of humanity
* Privacy Pass for the Accessibility use case.
* They do not want to track the user, and to link issuance and redemption.

In practice:

* They want to rate limit users. This is about metadata, and can be done based on IPs, ...
* One option is to use a risk score, so no Private identifiable information is leaked.

Security issues

* Can we really provide this service without keeping metadata?
* There are various classes of users, and we would reject PP for certain classes
  of users
* Segment blinded redemption to enforce security guarantee. This is a trade off
  with privacy offered by the blinded tokens

Considerations

* Don't want to maintain global double spend cache
* We need a minimal level of security
* How can we be efficient about key repudiation
* Browser integration is going to be required to have a better user flow and security guarantee

### Steven Valdez (Google Chrome Privacy Sandbox): Trust tokens API

This is Privacy Pass as a Web API (a Chrome Web API).

Goals

* For DDoS, bots, Spam protection

Application

* Antifraud, CAPTCHA
* No link between websites for Cross-site tracking

At Google

* Fraud and reCAPTCHA are looking at it
* They have 6 buckets of metadata, 2.5 bits

Standards

* Google is pushing W3C Web API.

Comments from Michele Orru:

Google (and Cloudflare) would like to get rid of the actual issuance and leave
it to third parties. This is critical and no company has enough trust in
external providers to do it.

We have an improvement with ZKP to protect the user privacy

### Subodh Iyengar (Facebook): PrivateStats

Privacy Pass style API in Whatsapp

Application

* Client side telemetry

Architecture

* Mobile app logs data
* Application server (has access to user authentication credential)
* Backend credential server, contains the VOPRF key and protects against double
  spend attack

Challenges

* Double spend
* Client app size
* Re-identification resistance via side channel
* Rate limit de-identified requests

In practice

* There are not much token reuse, and this is usually the case when the user has
  poor connectivity
* Only a small number of tokens per user, because they are linked to a private
  key. This is practical because they have a UID

New construction: AB-VOPRF

* Pairing Free
* Allow for faster key rotation
* 1.6ms proof and 0.2 without proof

### George Kadianakis (Tor project) - Insight from Tor

Their interest started years ago. This was to unblock people to access nodes.
There are DDoS against nodes and against the network itself.

They would like to:

* Rate limit people using anonymous credentials
* Have a better IP reputation from their exit nodes. This would improve user experience
* Strong unlinkability. The security assumption might be broken, but that should be good enough for rate limit
* Revocation to use the tokens for identity

They don't have the resources to look at it, and information is not easy to get.
The terms are not consistent across the literature.

### Gonçalo Pestana and Iñigo Querejeta (Brave) - Brave Rewards use case

This is built on Privacy Pass. This is used for BAT redemption.
The full model is on [GitHub](https://github.com/brave/brave-browser/wiki/Security-and-privacy-model-for-ad-confirmations).

Goals

* Ad interaction unlinkability
* Anonymous authorisation (for payment)

Two types of tokens

* Authentication
* Payment

They would like

* Verifiable attribute in the Anonymous credentials
* The issuer to delegate issuance to third parties (this is done by Google Trust token)

### Martin Strand (Norway Covid app) - How we use Privacy Pass

The blog is available [here](https://security.christmas/2020/21)

They would like:

* Public fields associated to the token

## Steps to take

* Create a second meeting closer to IETF110
* Come up with a SoK for Anonymous Credentials
