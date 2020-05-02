# Invites

Scaffold contains a rather simple invite system. Out of the box user's can invite people to Teams and maintain a membership on a team. However, this same invite code can be set up with other models, such as Documents or Pages. You would need to utilize the `HasMembers` and `Invitable` traits and write some custom parts but the core is in place.

## Traits

__^^HasMembers^^__

This handles the acts of inviting, leaving and removing a member from the model. This is added to the controller of the model you wish to invite users to.

__^^Invitable^^__

This handles the invites relationship to the model. This is added to any model you wish to invite a user to.

## Invite Notifications

A user can get two notifications that they have been invited to a team. If they are a user in the app they get an email (if they allow for email notifications) and an in-app notification. The invites page is the only way to accept invites.
The `UserInviteEmail` which is sent out contains two potential endpoints. If the user already exists then when they come to visit the site via the email they are directed to the invites page to accept the invite. If they are not a user they get sent to the registration page and once they have an account they are directed to the invites page.
The user who sent the invite also receives an in app notification that they sent an invite.

## Resending or Revoking

A user who sends invites has the option to resend or at any time revoke an invite. Once the invite has been accepted the invite is deleted and the user would have to be removed from the Team or other model.

## Accepting or Rejecting

A invitee has the option to accept or reject an invite.
