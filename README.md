# SR3Hystrix_EdgeService
Test project #2 for reproducing SC Brixton.SR3 issues with Hystrix.

## Reproducing Issue

. Run LOTE_ConfigService
. Run LOTE_EurekaService
. Run test project #1 (SR3Hystrix_QuoteService)
. Run this project
. Kill quote-service (test project #1 above, SR3Hystrix_QuoteService)
. Update entry for quote in edge-service.properties (bootiful-microservices-config feeds the config-service)
. git commit -a -m "Changed quote"
. From terminal, curl -d{} localhost:8086/refresh
. Verify updated quote provided by edge-service
. Restart quote-service
. If running SR3, wait as long as you like; the circuit never closes (at least it didn't on my machine)

Changing the edge-service POM to use Brixton.RELEASE eliminates the issue. I haven't yet tested with SR1 or SR2.

It's quite possible the config change & refresh contribute nothing to the discussion, but I included them for completeness, as these are the steps I was executing.

