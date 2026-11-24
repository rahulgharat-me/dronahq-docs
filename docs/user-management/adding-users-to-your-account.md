---
sidebar_position: 1
title: Adding users to your account
---

import Thumbnail from '@site/src/components/Thumbnail';
import VersionedLink from '@site/src/components/VersionedLink';

# Adding users to your account

You can add users in DronaHQ using the following 4 ways:

- Send invites to your users to sign-in or sign-up with their email and password
- Auto activate users with a preset password (Users can always change their passwords in the end-user portal) 
- Using SSO and JIT (Just In Time) user addition
- By configuring SCIM to auto provision users from an IDP

:::caution Please Note

You you need to have admin or owner level privileges to access or modify these configurations.

:::

## Send invites to users 

You need to Navigate to `Manage Users -> Add Users` where you can invite multiple users at a time. These invited users will get an email with the instructions to verify and activate their account and create a password. After activating their account, depending on the role of the user they can either login to the `end-user portal` or/and the `creator portal` instructions and details of which are sent on the invite email itself. 

You need the following details to invite users:
- User name
- Email
- Password (Optional - only if `Auto-activate` is selected)
- User Group
- [User Role](/user-management/user-roles/)

<figure>
  <Thumbnail src="/img/user-management/add-users-wo-password.png" alt="Inviting users" width='100%'/>
  <figcaption align = "center"><i>Invite users</i></figcaption>
</figure>

:::info Please Note

In case the invite email doesn't reach the user for whatever reasons, the admin can check the invited user in the `Manage Users` section and click on `Activate Link` to view it and manually send the user the activation link.

:::

## Auto activate users with a password

In DronaHQ you can also Auto-activate users instead of inviting and sending them an activation email. This method should generally be avoided as it can potentially be insecure with pre-set passwords. It should only be used in specific cases where invite emails can't be sent for various reasons. 

<figure>
  <Thumbnail src="/img/user-management/add-users-with-password.png" alt="Auto activate users" width='100%'/>
  <figcaption align = "center"><i>Auto activate users</i></figcaption>
</figure>

:::caution Manual emails

Since these users are `Auto-activated` you will need to manually inform them or email them the login links etc. We provide an email template which you can use for sending out these emails manually to your users.

:::

## Using SSO and JIT

DronaHQ supports SSO on both cloud and self-hosted editions. You can configure any of the following types of SSOs with the IDP of your choice like Okta. 

- [SSO using OIDC](/sso/configuring-sso-with-oidc)
- [SSO using SAML](/sso/configuring-sso-with-saml)
- [SSO using custom JWT](/sso/configure-sso-with-custom-jwt)
- [SSO using Google OIDC](/sso/configuring-sso-for-google-oidc) 
- Sign in using Google (`DronaHQ Cloud edition` only)

Once the SSO is configured the users can login directly without having a separate email and password for DronaHQ. 

<figure>
  <Thumbnail src="/img/user-management/add-users-sso.png" alt="Adding users using SSO" width='100%'/>
  <figcaption align = "center"><i>Add users using SSO</i></figcaption>
</figure>

:::tip Onboarding SSO users from your own admin app (`DronaHQ Self-Hosted edition` only)

You can also create SSO users programmatically, with their groups pre-assigned, using [SSO mode on the Users REST API](/self-hosted-deployment/users-rest-api/create-users#sso-mode). This is useful when you want to onboard all of your users from your own admin application instead of from the **Manage Users** screen.

:::

## Auto provision users using SCIM

DronaHQ supports SCIM, known as the groundbreaking System for Cross-domain Identity Management, has emerged as the undisputed industry leader in effortlessly provisioning user accounts. This invaluable solution proves its worth, especially in scenarios where enterprises entrust third-party SSO providers like Okta or Azure Active Directory to oversee employee authentication. Thanks to SCIM's extraordinary capabilities, user accounts undergo automatic creation, seamless updates, and streamlined deactivation across various applications, revolutionizing the way organizations manage their workforce access.

[Read more about SCIM](/sso/scim-user-provisioning)