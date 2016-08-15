# SR3Hystrix_EdgeService
Test project #2 for reproducing SC Brixton.SR3 issues with Hystrix.

## Reproducing Issue

* Clone https://github.com/hecklerm/bootiful-microservices-config (to feed the config server) and next four projects listed below
* Run SR3Hystrix_ConfigService (https://github.com/hecklerm/SR3Hystrix_ConfigService) from within IntelliJ
* Run SR3Hystrix_EurekaService (https://github.com/hecklerm/S3Hystrix_EurekaService) from within IntelliJ
* Run SR3Hystrix_QuoteService (https://github.com/hecklerm/SR3Hystrix_QuoteService) from within IntelliJ
* Run this project (SR3Hystrix_EdgeService) from within IntelliJ
* Go to http://localhost:8086/quote to use Zuul proxy to hit local (edge-service) endpoint that, via a @LoadBalanced RestTemplate @Bean referencing quote-service by name (Eureka lookup), accesses the quote-service's /random endpoint. This is convoluted, but it falls within documented uses and demonstrates several interactions among the Netflix components (Zuul, Eureka, Hystrix, & behind the scenes, Ribbon). Refresh a few times to see that verify random quote is being returned each call (may or may not be different, after all, it's random)  ;)
* Kill quote-service (SR3Hystrix_QuoteService)
* Refresh browser to see that the Hystrix fallbackMethod is being hit, a new Quote is being created from edge-service.properties' quote property (defined at bottom of file)
* Update the entry for quote in edge-service.properties and save the file
* git commit -a -m "Changed quote"
* Go to http://localhost:8086/quote to demonstrate that the bean value hasn't yet been refreshed (still original edge-service.properties quote, buffered)
* From the terminal, curl -d{} localhost:8086/refresh to force a bean refresh
* Go to http://localhost:8086/quote to verify updated quote is now provided by edge-service
* Restart quote-service
* Go to http://localhost:8086/quote to verify that the circuit never closes (duplicated on two machines, but only if this exact sequence is followed)

NOTE: If I remove the HATEOAS dep, everything seems to work properly, even with the same sequence of steps/events.

Please ping me with any questions. Happy to pair to demonstrate if desired.
