# Scaffold Overview

Grafite Scaffold provides an elegant solution for starting an application by having the most basic views, controllers, models, migrations, packages and other features built in advance. It is a fine tuned boilerplate for any client application you may need to develop. Below we'll cover all the basic additions that are not part of a fresh Laravel application.

## PHP Code

### Dependencies

Please consult documentation related to these packages if you wish to work with their functionality.

[consoletvs/charts](https://github.com/ConsoleTVs/Charts) <br>
[grafite/database](https://github.com/GrafiteInc/Database) <br>
[grafite/formmaker](https://github.com/GrafiteInc/FormMaker) <br>
[intervention/image](https://github.com/Intervention/image) <br>
[laravel/cashier](https://github.com/laravel/cashier) <br>
[laravel/helpers](https://github.com/laravel/helpers) <br>
[laravel/ui](https://github.com/laravel/ui) <br>
[lasserafn/php-initial-avatar-generator](https://github.com/lasserafn/php-initial-avatar-generator) <br>
[tightenco/ziggy](https://github.com/tightenco/ziggy) <br>

<hr>

### Charts
*ActivityThirtyDays*

Builds a chartJS driven chart showing the count of all user activity of the course of 30 days. The chart is visible in the Admin Dashboard.

*RegistrationThirtyDays*

Builds a chartJS driven chart showing the count of all user registrations of the course of 30 days. The chart is visible in the Admin Dashboard.

<hr>

### Exceptions

__^^Handler^^__

We made some modifications to the Exception handler allowing us to enforce JSON responses from API calls, as well as a general error page for exceptions not caught by the error number pages.

<hr>

### Helpers
__^^ActivityHelper^^__

The activity helper is in place to call anywhere in your application that you want to log a user activity.

__^^NotificationHelper^^__

We added notification helpers in order to avoid writing multiple lines of code for every app notification and email notification

__^^UserInterfaceHelper^^__

The primary benefit of the UserInterfaceHelper was having a place to put the method for helping determine which route links should be highlighted as active.

<hr>

### Controllers

In general all controllers added are kept to a minimum. We opted to section the scaffold app with API and Ajax controller directories. This is because the API in this app uses the API auth, and has the intention of being external. Given that we have various axios calls in the Vue components, and since those use the standard Laravel auth, it felt more natural to title them as Ajax implying their internal app nature.

__^^DashboardController^^__

The dashboard controller is the most basic one, after login its the general home page for the application.

__^^InvitesController^^__

The invites controller handles the revoking and resending of invites, the sending of invites is performed by the `InviteService`

__^^PagesController^^__

The pages controller is intended for general app pages like terms of service and contact.

__^^TeamMembersController^^__

Team members controller handles the basic CRUD pages of a team member including: show, edit, update. Any other actions relevant to team members is handled by the `HasMembers` trait.

__^^WebhookController^^__

This is an extention of the Cashier webhook controller setting the redirect of a successful payment.

<hr>

#### Concerns

The concerns directory contains any trait files.

__^^HasMembers^^__

This trait handles all method relavant to being or having members on a model. By default this is only present on Teams. It contains methods handling: leaving a team, removing a member, and inviting a member.

<hr>

#### Admin

All Admin level controllers require having the `admin` role.

__^^DashboardController^^__

The Dashboard controller loads the charts mentioned above for a basic overview of the app user state in the admin dashboard.

__^^RoleController^^__

A CRUD controller for the application roles and permissions. Permissions are defined in the permissions config file, but roles are dynamic.

__^^UserController^^__

A CRUD controller for app Users. When editing a user the Admin will be able to see their activity records and be able to login as them, for testing purposes etc.

<hr>

#### Ajax

Ajax controllers are intended to handle any type of ajax calls within the application since they use the standard Laravel auth.

__^^ApiTokenController^^__

The API token controller handles the reset of a user's API token.

__^^BillingController^^__

The billing controller takes care of the creating and updating of payment methods for subscriptions.

__^^CookiePolicyController^^__

The cookie policy controller handles the acceptance of the cookie policy for a user.

__^^NotificationsController^^__

The notifications controller has the notification count for a user which is what lets us show the count of notifications in the navigation bar.

<hr>

#### API

Using Laravel's basic auth for the API lets us have users create an encrypted token.

__^^ApiController^^__

This is a base controller for the API. It handles setting a `user` property coming from the Laravel basic auth.

__^^UsersController^^__

The users controller handles getting a users info, updating it, and deleting the user model.

#### Auth

Nearly all auth controllers in Scaffold are the standard ones from a fresh Laravel app.

__^^RegisterController^^__

There are two main modifications here, one sets the newly created user's role. The other hanldes the registration via invites.

__^^RegisterViaInvites^^__

Registraton via invites handles the landing page provided by the invitation link as well as its validation method.

#### User
BillingController
DestroyController
InvitesController
NotificationsController
SecurityController
SettingsController
TeamsController

### Forms
**AdminUserForm**


**InviteUserForm**


**RoleForm**


**TeamForm**


**TeamInviteForm**


**TeamMemberForm**


**UserForm**


**UserSecurityForm**



### Fields
**ToggleField**

### Middleware
**Activity**


**Admin**


**Permissions**


**Roles**


**Subscription**



### Requests
AdminUserUpdateRequest
ApiUserUpdateRequest
PasswordUpdateRequest
TeamCreateRequest
TeamUpdateRequest
UserUpdateRequest

### Resources
UserResource

### Models
Activity
Invite
Role
Team
User
##### Concerns
HasActivity
HasPermissions
HasRoles
HasSubscription
HasTeams
Invitable
Searchable

### Notifications
InAppNotification
StandardEmail
InviteUserEmail

### Services
ActivityService
InviteService
TeamService

## JavaScript Code

### Dependencies

Please consult documentation related to these packages if you wish to work with their functionality.

@dashboardcode/bsmultiselect
@grafite/helpers
animate.css
bootstrap
bootstrap4-toggle
vue-clipboards
vue-cookie-law
vue-snotify


### Vanilla JS
sidebar
subcription-create
subcription-payment-method

### Vue Components
api-token
cookie-law
copy-button
modal
notification-badge
notification-marker
session

