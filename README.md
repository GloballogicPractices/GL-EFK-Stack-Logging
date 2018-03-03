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
 
	* (1) `source`: where all the data come from 
	
			Fluentd’s input sources are enabled by selecting and configuring the desired input plugins 
			using source directives. Fluentd’s standard input plugins include http and forward. 
			http turns fluentd into an HTTP endpoint to accept incoming HTTP messages whereas 
			forward turns fluentd into a TCP endpoint to accept TCP packets. Of course, 
			it can be both at the same time (You can add as many sources as you wish)
			
	* (2) `match`: Tell fluentd what to do! 
		
			The “match” directive looks for events with matching tags and processes them. 
			The most common use of the match directive is to output events to other systems (
			for this reason, the plugins that correspond to the match directive are called “output plugins”). 
			Fluentd’s standard output plugins include file and forward.Each match directive must include a match pattern and a @type parameter. 
			Only events with a tag matching the pattern will be sent to the output destination (in the above example, 
			only the events with the tag “myapp.access” is matched). The @type parameter specifies the output plugin to use.
				
	* `filter`: Event processing pipeline 

			The “filter” directive has same syntax as “match” but “filter” could be chained for processing pipeline. Using filters
			Received event, {"event":"data"}, goes to record_transformer filter first. record_transformer adds “host_param” field 
			to event and filtered event, {"event":"data","host_param":"webserver1"}, goes to file output.

	* Set system wide configuration: the `system` directive
			
			Following configurations are set by system directive. You can set same configurations by fluentd options:

				log_level
				suppress_repeated_stacktrace
				emit_error_log_interval
				suppress_config_dump
				without_source
				process_name (only available in system directive. No fluentd option)
		
	* Group filter and output: the `label` directive
	
			The “label” directive groups filter and output for internal routing. “label” reduces the complexity of tag handling. 
			Here is a configuration example. “label” is built-in plugin parameter so @ prefix is needed.
			

	* Re-use your config: the `@include` directive
		
			Directives in separate configuration files can be imported using the @include directive:
			The @include directive supports regular file path, glob pattern, and http URL conventions:
			Note for glob pattern, files are expanded in the alphabetical order. If you have a.conf and b.conf, 
			fluentd parses a.conf first. But you should not write the configuration depends on this order. 
			It is so error prone. Please separate @include for safety
			
	
 
 * #### Creating log pattens
 
	* use Rubular.com to create custom logging par
			
	* Configuring/Installing StackStorm Packs
		
			After Installation use st2 utility to install required packs
			st2 pack install rabbitmq
			st2 pack config rabbitmq
			enter your cred's and queue name
			git clone https://github.com/GloballogicPractices/GLAuto-Remedial-Stack.git
			st2 rule add -f GLAuto-Remedial-Stack/stackstrom/rules/remedy.yaml
			st2 rule add -f GLAuto-Remedial-Stack/stackstrom/rules/summary.yaml	
				
	* Installing RunDeck/CLI - Same as public cloud

