## How to test websevices in the 2020s?
To my knowledge there is no one way to test a webservice today, however there are pointers to what direction you should move. This is merely a list of my thoughts based on what I’ve seen the last couple of years. See this list as a baseline for possible quality efforts to inject in your pipelines.

### General
* As a complement to functional tests, measure: with some few fantastic key metrics or checks/verifications you can observe the continuous functionality of your service during continuous deployment. (find that 'etsy tech talk')
* Think through how you get prod-data in test environments, you might need to anonymise this.
* Make sure to have a db-backup schema. Maybe also a db-snapshot schema.
* How do you ensure consistent functionality when cloud-scaling your service? One db of truth, per-user-geo-relinking based on UID, a cloudy-database (often very slow), etc.
* How do you run end-to-end tests on your service?
* A few extensive integration tests can replace huge amounts of code-specific unit tests.
* Don’t like to document stuff (then practice)? Do it in your codebase using an [ArchitecturalDesignRecord](https://github.com/joelparkerhenderson/architecture_decision_record).
* To move quick: make sure tickets have a clear Definition of Done (DoD), are getting code-reviewed by the team, have all manual test information in the ticket, and are getting manually tested before being pushed upstream. To move slow: Plan testing for the last week before public release.
* Allocate time to regularly upgrade 3rd party libraries and taking care of your technical heritage.
* Make sure logs are in place for the full stack and make them easy to access to make troubleshooting easy.
* Before you start a project, make sure all developers are aware of [OWASP top ten list](https://owasp.org/www-project-top-ten/) 

### UI
* Using Selenium with headless Chrome to verify UI endpoints is the old way of doing web testing. It is also the most used and most standardised and will probably be a secure path forward.
* If you rely extensively on the UI being correct, start looking at UI testing tools like [Wraith(https://github.com/BBC-News/wraith)] or Jest or such.  (extremely expensive)
* If you want to be the kool kid, use [Jest](https://jestjs.io/) (framework designed for testing React) or [Puppeteer](https://github.com/puppeteer/puppeteer) (Google's own chrome-test-framework)
* JavaScript development culture is borked, which is fine as long as you make sure to use only secure stuff (like if that would be possible). Run vulnerability checkers continuous on master, today's flavour might be: https://snyk.io/

### Rest of the stack
* Make sure to have full API tests in place (i.e. verify all the APIs responses): Relying on your API is a very good feeling.
* I have never seen well functioning automagick testing of APIs, but that is probably a future: [fuzzying](https://patricegodefroid.github.io/public_psfiles/icse2019.pdf) or [test case generation](https://arxiv.org/pdf/1901.01538.pdf); but be prepared for slowing down development and test speed.
* Version your API Pick a [schema](https://www.xmatters.com/blog/devops/blog-four-rest-api-versioning-strategies/) for versioning and when to bump versions.
* Design for surviving spammy behaviour:
* At first implement a rate limit schema for your API (please don’t build a service that can be taken down by a cat sleeping on the space bar).
* Look into ways to design your system which don’t rely on end-user-polling of endpoints. 
* Build for loose integration to your database, do not design your service in the database (unless you really have to, but even then you probably shouldn’t)
