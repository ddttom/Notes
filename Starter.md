Starter

Always have a design rules, whitelabelable. Button shapes sizes. Breakpoints.

Coding standards

Simple plain javascript. Do not add third party  libraries without the users explicit permission

Css 3 with variable, master styles. Block level additions and changes. Block level must know about master.

Config variables at top of code. No hard coded text or constants.

Commenting rules must be added

Look for opportunity to add to core library when functions are shared. Simplicity over complexity. Maintanability over clever code.

Create two folders. Frontend and backend.

Create message q folder, msg q implementation should be graphql with action commands. The front end should not have knowledge of which connectors are in use, just knows that connectors are available and the expected schema

The backend should not know what the front end will do with result. It strictly follows the schema. The schema index is where the developer and the AI collaborate to  build the APIs, the msq q and the connectors.

Create api folder

We want to avoid sql migration problems use a no-sql dB approach

Consider a Refactoring command

Attribution in git

Core library is shareable between back end  and front end

Use EDS shim

Add plusplus to shim.