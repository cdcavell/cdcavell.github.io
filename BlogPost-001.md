## Net Core MVC OpenIdConnect Authentication
### New User Registration

Interesting scenario I faced recently, develop a process that authenticates a user; check for an existing registration; and if not found, redirect to registration process. I solved this with a combination of OpenIdConnect OnTicketReceived event handler along with Authorization Policies.

In `Startup.cs` `ConfigureServices` method, I have a call to the `OnTicketRecieved` handler in the `Events` option of the `AddOpenIdConnect` option of `services.AddAuthentication`. This allows me to inspect the `Identity` `Claims` sent from IdP and add additional claims to `Identity` before further pipeline processing. I am able to get the `dbContext` from `HttpContext` and perform a lookup to check for existing registration record based on retrieved email claim type. If found I set a custom claim type “registration” to “existing” otherwise I set to “new”. This custom claim type is then added to `Identity` `Claims` and `Response` is sent on through pipeline for further processing.
