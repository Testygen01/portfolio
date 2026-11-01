# Diagnosing an MFA Browser Compatibility Issue
`MFA` `FIDO2` `Authentication` `Customer Support`

## Overview

A user on macOS could not complete Passwordless authentication in Firefox. The credentials were valid and the user had hardware-based authentication available, but the login would not finish. The cause was a documented browser limitation, not anything wrong with the user's account or devices, and the fix was guidance, not a configuration change.

## The problem

The failure was specific to the browser. The user had Touch ID and a security key available on macOS, and both should have worked for Passwordless login. In Firefox, neither did. That pattern pointed away from the account or the authenticator and toward how Firefox handled the authentication flow on macOS.

## Investigation

I traced the login attempt to the exact point of failure: the step where the browser hands off to the hardware authenticator. Per Duo's documentation, Firefox on macOS does not support Touch ID because it lacks the necessary FIDO2 support, and it cannot prompt for the security key's PIN to satisfy the Passwordless user-verification requirement. On macOS, Firefox only supports Duo Push for Passwordless authentication. That confirmed the issue was browser-dependent. The user's credentials, security key, and enrollment were all fine; the browser simply could not complete the ceremony the login required.

## Resolution

With the cause confirmed in Duo's documentation, I explained the limitation in plain terms and guided the user to authenticate with Duo Push, the supported Passwordless method in Firefox on macOS. The user had walked in thinking their key or account was broken, and the fix needed them to understand that neither was true. No account changes, no re-enrollment, no weakening of the security posture.

## Result and scope

The user was unblocked in a single support interaction with no backend intervention. The case never needed to leave the tier where it arrived; it needed someone to identify that the browser, not the identity, was the problem, and to translate that finding into an action the user could take immediately.

## What this shows

This case shows authentication troubleshooting that separates the layers. Credentials, authenticator, and browser were three different suspects, and the evidence pointed at the browser. It also shows the value of checking vendor documentation before acting: without the Duo article confirming the Firefox on macOS limitation, the easy move would have been churning the user's account settings while the real cause sat untouched in the browser.
