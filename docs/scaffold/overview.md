# Scaffold Overview

Grafite Scaffold provides an elegant solution for starting an application by having the most basic views, controllers, models, migrations, packages and other features built in advance. It is a fine tuned boilerplate for any client application you may need to develop. Below we'll cover all the basic additions that are not part of a fresh Laravel application.

## PHP Code

### Dependencies

Please consult documentation related to these packages if you wish to work with their functionality.

[grafite/auth](https://github.com/GrafiteInc/auth){:target="_blank"}<br>
[grafite/charts](https://github.com/GrafiteInc/Charts){:target="_blank"}<br>
[grafite/database](https://github.com/GrafiteInc/Database){:target="_blank"}<br>
[grafite/forms](https://github.com/GrafiteInc/Forms){:target="_blank"}<br>
[grafite/html](https://github.com/GrafiteInc/Html){:target="_blank"}<br>
[grafite/support](https://github.com/GrafiteInc/Support){:target="_blank"}<br>
[intervention/image](https://github.com/Intervention/image){:target="_blank"}<br>
[laravel/cashier](https://github.com/laravel/cashier){:target="_blank"}<br>
[laravel/helpers](https://github.com/laravel/helpers){:target="_blank"}<br>
[lasserafn/php-initial-avatar-generator](https://github.com/lasserafn/php-initial-avatar-generator){:target="_blank"}<br>
[tightenco/ziggy](https://github.com/tightenco/ziggy){:target="_blank"}<br>

<hr>

### Charts
__^^ActivityThirtyDays^^__

Builds a chartJS driven chart showing the count of all user activity of the course of 30 days. The chart is visible in the Admin Dashboard.

__^^RegistrationThirtyDays^^__

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

Team members controller handles the basic CRUD pages of a team member including: show, edit, update.

__^^WebhookController^^__

This is an extention of the Cashier webhook controller setting the redirect of a successful payment.

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

<hr>

#### Auth

Nearly all auth controllers in Scaffold are the standard ones from a fresh Laravel app.

__^^RegisterController^^__

There are two main modifications here, one sets the newly created user's role. The other hanldes the registration via invites.

__^^RegisterViaInvites^^__

Registraton via invites handles the landing page provided by the invitation link as well as its validation method.

<hr>

#### User
__^^BillingController^^__

The billing controller contains all get routes for elements pertaining to billing. It also contains the coupon methods and invoice getters.

__^^DestroyController^^__

This controller is a dedicated delete controller for the user.

__^^InvitesController^^__

The invites controller handles all aspects of user invites including: listing, accepting and rejecting.

__^^NotificationsController^^__

Handles the CRUD of in app notifications.

__^^SecurityController^^__

Handles the password updating for a given user.

__^^SettingsController^^__

For updating user settings.

__^^TeamsController^^__

Handles Team CRUD elements relative to how a user would own a team.

<hr>

### Forms

Scaffold uses the Grafite Form Maker so you can easily manage app forms via PHP Form Classes.

__^^AdminUserForm^^__

The form for when admin's manage users.

__^^InviteUserForm^^__

The base form for inviting users.

__^^RoleForm^^__

The role CRUD form.

__^^TeamForm^^__

The team CRUD form.

__^^TeamInviteForm^^__

The base form for inviting people to teams.

__^^TeamMemberForm^^__

The team member CRUD form.

__^^UserForm^^__

The user CRUD form.

__^^UserSecurityForm^^__

The user password update form.

<hr>

### Fields

__^^ToggleField^^__

The toggle field sets the checkbox attributes enabling it to work with <a target="_blank" href="https://gitbrent.github.io/bootstrap4-toggle/">bootstrap4-toggle</a>.

### Middleware

There are a few custom middleware classes added, and most revolve around roles and permissions.

__^^Activity^^__

The activity middleware will log a request as an activity for a user. This can be overkill for some apps, so feel free to remove it from the web routes.

__^^Admin^^__

The admin middleware checks if the user has the admin role.

__^^Permissions^^__

Permissions middleware requires a permission key, and then checks if the user accessing the route has the permission based on their role.

__^^Roles^^__

Roles middleware is the same as admin in that it checks for the user's role and confirms or rejects the request.

__^^Subscription^^__

Subscription middleware confirms that the user has an active subscription before proceeding.

### Requests

Most requests simply use the corresponding model's rules.

__^^AdminUserUpdateRequest^^__

Authorization requires admin role and sets the rules.

__^^ApiUserUpdateRequest^^__

Confirms the user is authorized by the API basic auth.

__^^PasswordUpdateRequest^^__

Confirms matching passwords.

__^^TeamCreateRequest^^__

Follows rules relevant to teams

__^^TeamUpdateRequest^^__

Follows rules relevant to teams

__^^UserUpdateRequest^^__

Confirms user is logged in and sets rules for email, name and avatar.

<hr>

### Resources

__^^UserResource^^__

The user resource is used for setting the `visible` attributes of a user for access in JavaScript code via Vue Components etc.

<hr>

### Models

__^^Activity^^__

The activity model.

__^^Invite^^__

The invite model.

__^^Role^^__

The role model.

__^^Team^^__

The team model.

__^^User^^__

The user model.

<hr>

#### Concerns

__^^HasActivity^^__

Has activites contains the activities relationship.

__^^HasPermissions^^__

The has permissions trait contains the attribute setter for permissions. This is the method which scans the user roles and collects the permissions that are applicable to the user.

__^^HasRoles^^__

The roles trait contains the relationships of roles to the model.

__^^HasSubscription^^__

The subscription trait contains extra methods pertaining to active vs cancelled subscriptions as well as the `subscriptionPlan` method.

__^^HasTeams^^__

This trait contains the teams relationship (teams the user owns) and team memberships (teams the user is a memeber of).

__^^Invitable^^__

The invitable trait contains the invites relationship and the `invite` creation method itself.

__^^DatabaseSearchable^^__

The database searachable trait provides a simple search method which creates a query to look at each field of the model and search for the keyword. This is a simple use case tool, which when scale calls for it we recommend replacing with `laravel/scout`.

<hr>

### Notifications

__^^InAppNotification^^__

The in app notifications is a handy notification to create a notification in the database, which the user can mark as read, or delete.

__^^StandardEmail^^__

When using the standard email notification the end user gets a standard email. This basic email has a `subjhect`, `greeting`, `message` and a login button.

__^^InviteUserEmail^^__

The invite user email has a basic email but it includes a token for account activation and message for the user's invite.

<hr>

### Services

__^^ActivityService^^__

The activity service handles extracting the key data points from the requests in order to log them into the activities table.

__^^InviteService^^__

The invite service handles the creation of invites and the validation of invites.

__^^TeamService^^__

The team services handles the basic CRUD actions of a team as well as leaving, adding and removing members. This service handles things like sending out extra notifications and the authorizing of these actions.

<hr>

## JavaScript Code

<hr>

### Dependencies

Please consult documentation related to these packages if you wish to work with their functionality.

[@dashboardcode/bsmultiselect](https://github.com/DashboardCode/BsMultiSelect){:target="_blank"}<br>
[@grafite/helpers](https://github.com/GrafiteInc/Helpers){:target="_blank"}<br>
[animate.css](https://daneden.github.io/animate.css/){:target="_blank"}<br>
[bootstrap](https://getbootstrap.com/){:target="_blank"}<br>
[bootstrap4-toggle](https://gitbrent.github.io/bootstrap4-toggle/){:target="_blank"}<br>
[vue-clipboards](https://www.npmjs.com/package/vue-clipboards){:target="_blank"}<br>
[vue-cookie-law](https://www.npmjs.com/package/vue-cookie-law){:target="_blank"}<br>
[vue-snotify](https://github.com/artemsky/vue-snotify){:target="_blank"}<br>

<hr>

### JavaScript Components

__^^sidebar^^__

The sidebar quite literally handles the sidebar hiding and revealing based on the size of the window.

__^^subcription-create^^__

Handles the create subcription request handling for Stripe integration.

__^^subcription-payment-method^^__

Handles the changing of the payment method for Stripe integration.

### Vue Components
__^^api-token^^__

The API token provides a disabled input field with the current user token when its reset. Scaffold by default encrypts tokens so they're only visible after resetting them. Afterward they are no longer visible.

__^^cookie-law^^__

The `cookielaw` component handles a basic footer popup that indicates the app uses cookies. This can be customized as needed.

__^^copy-button^^__

The copy button accepts a message argument for what in fact will be copied when the button is clicked.

__^^modal^^__

A handy modal which can be used anywhere in the app that you have a form submission to draw a modal on screen with a message. It uses `window.confirmation(this, "message")` to trigger it. For example:

```html
<button type="submit" onSubmit="confirmation(event, 'this is a modal')">Submit</button>
```

__^^notification-badge^^__

The notification badge vue component provides a number of the unread messages for the in app notfications

__^^notification-marker^^__

The notification marker vue component provides a color marker indicating that the user should click on their notifications link.

__^^session^^__

The session vue component integrates with Snotify handling notifications on requests. If the request or redirect comes with a `message` or `errors` this vue component will trigger Snotify.
