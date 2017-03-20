# playbook-folder/roles/internal/blank/defaults

This is the place to put default variables.

So if there are typical defaults - aka apache uses port 80 - then if your conf file has a space for to specify the port number, you would reference a variable that is defined here.

Keep in mind that when others are looking at this role on github or ansible-galaxy, this is a very important place to understand what's going on with the role itself, as well as what parameters specific variables are expecting.

It _is_ ok to not define some of the variables here, especially if they're ones that can be gotten either from the node after gathering it's info, or if it's a very personalized variable - like a name. It should be made clear that other variables are needed if they are - but the "sane defaults" should be put here.

This file is the lowest in the food chain. This is overridden by variables set almost anywhere else - including in the tasks themselves. So if you're doing something that isn't a widely-know best-practice why-would-you-do-it-any-other-way it's probably best to leave it up to the admin to define it.
