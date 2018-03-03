## EFK Centerlized Logging Stack for AWS Cloud

This stack allows you to impliment centerlized logging on your infrastructure.

## Pre-Requisite
Before using this stack you need to have below things already in your infrastructure.

* #### Public Cloud (AWS)
	##### Public cloud recovery flow
		td-agent(client) -> td-agent(server) -> Elasticsearch -> Kibana/Grafana 

	* Hosted EC2 instances
	* td-agent server in clusterd mode, or behind ELB
	* fluentd plugins
	* AWS Elasticsearch domain, of self hosted elasticsearch server endpoint
	* Kibana/Grafana for visulizing the logs

### td-agent config explained

 * #### List of Directives
 
	* (1) “source”: where all the data come from 
	
			Fluentd’s input sources are enabled by selecting and configuring the desired input plugins using source directives. Fluentd’s standard input plugins include http and forward. http turns fluentd into an HTTP endpoint to accept incoming HTTP messages whereas forward turns fluentd into a TCP endpoint to accept TCP packets. Of course, it can be both at the same time (You can add as many sources as you wish)
			
	* match 
		
			directives determine the output destinations.
				
	* filter 

			directives determine the event processing pipelines.

	* system 
			
			directives set system wide configuration.
		
	* label 
	
			directives group the output and filter for internal routing
			

	* @include
		
			directives include other files.
			
				
	* CloudWatch Plugins

		* You can use your own CloudWatch plugins for this stack
		* Make sure that Alert name should be in the above mentioned format.
	
	
 
 * #### td-agent server config exaplained
 
	* Configuring/Installing StackStorm  - Same as public cloud
			
	* Configuring/Installing StackStorm Packs
		
			After Installation use st2 utility to install required packs
			st2 pack install rabbitmq
			st2 pack config rabbitmq
			enter your cred's and queue name
			git clone https://github.com/GloballogicPractices/GLAuto-Remedial-Stack.git
			st2 rule add -f GLAuto-Remedial-Stack/stackstrom/rules/remedy.yaml
			st2 rule add -f GLAuto-Remedial-Stack/stackstrom/rules/summary.yaml	
				
	* Installing RunDeck/CLI - Same as public cloud
			
	* Configure RunDeck - Same as public cloud

	* Installing Redmine - Same as public cloud
					
	* Installing RabbiMQ

			Use below link to install RabbitMQ
				https://www.rabbitmq.com/install-rpm.html
		
	* Installing Prometheus

			Use below link to install Prometheus
				https://prometheus.io/docs/prometheus/latest/installation/
			
	* Installing Alerta

			Alerta installation
				http://alerta.readthedocs.io/en/latest/quick-start.html
				https://github.com/alerta/alerta-contrib/tree/master/plugins/amqp
			
	* Prometheus Plugins
		* You can use official prometheus plugin
		* Or use metrics.sh for customize plugin
		
