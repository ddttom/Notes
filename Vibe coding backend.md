Vibe coding - backend

Make sure backend is highly modular and intermediate through a simple message queue with a very structured message api. Design this before doing any backend.

All backend code must be thread safe, injection proof and may share core library with front end

Backend should be multitenant and UI agnostic. Communicating with the message api with structured json.

Each tenant has a separate dB. Avoiding leakage. Each tenant has a global Config json object. Used for Config and overriding whitelabel

The initial build of the prototype us to be dummy backend functions that return this is a dummy - to be implemented. The backend should have the current full set of actions in a schema. The index is the schema. The initial implementation is returning "to be implemented" each action should be marked #TODO.  Each api/connector/agent will log all errors to a central log. All action message will be logged to a tenant log. The tenant log should be cycled at midnight and retained for one week.

The connector must use a data transformation object so that the connector can be traced and debugger.

All actions must be checked for sql injections or bad actor intents

Ensure that admin functions can only be used by the tenant owner, there should be possibilities of multiple owners per tenant. There will be one admin owner who can delegate access to secondary owners.

The developer admin can be promoted to super admin on any tenant and perform the important tasks, with full audit logging to the admin log.

A separate action channel should be created for the tenant and the developer allowing administration commands and dashboard. Initial implementation is to be as dummy with todo

the developer will focus one by one on each backend function. And send separate instructions

There must be an index of backend functions and the state and of each, dummy, erroring, under Dev, potential release, under user test and release., also reverted.

Connectors are used by by the backend, not the front end. Connectors can only be executed if the tenant has subscribed to the connector. The connector will return a not subscribed error, this should be passed back to the caller. These functions must be described in the index. The developed will have a route for handling not subscribed and will produce better messaging to the end usrr

The system must track and release these functions according to the users request. Create a folder of potential backend and a build process that allows the developer to select what will be released to the environment. We will have local Dev. Local test, local qa. Hosted test. Hosted live.