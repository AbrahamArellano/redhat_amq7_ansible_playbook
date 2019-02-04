- Run installation:

sudo ansible-playbook -vvv --vault-id @prompt -i hosts.yml amq-install.yaml

- Run uninstall:

sudo ansible-playbook --vault-id @prompt -i hosts.yml amq-uninstall.yaml


- Create queue:

	$AMQ_MASTER_HOME/bin/artemis queue create --auto-create-address --address dzbank.queue.test --name dzbank.queue.test --preserve-on-no-consumers --durable --anycast --user fappricing --password fappricing --url tcp://127.0.0.1:61616

- Producer:

	+ Master:

		$AMQ_MASTER_HOME/bin/artemis producer --message-count 10 --url tcp://127.0.0.1:61616 --destination queue://dzbank.queue.test --user fappricing --password fappricing
		
	+ Slave:
		
		$AMQ_SLAVE_HOME/bin/artemis producer --message-count 10 --url tcp://127.0.0.1:62616 --destination queue://dzbank.queue.test --user fappricing --password fappricing
		
- Consumer:

	+ Master:
	
		$AMQ_MASTER_HOME/bin/artemis consumer --message-count 100 --url tcp://127.0.0.1:61616 --destination queue://dzbank.queue.test --sleep 1000 --verbose --user fappricing --password fappricing
	
	+ Slave:
	
		$AMQ_SLAVE_HOME/bin/artemis consumer --message-count 100 --url tcp://127.0.0.1:62616 --destination queue://dzbank.queue.test --sleep 1000 --verbose --user fappricing --password fappricing
		
		
- Test:

	./artemis queue stat --queueName dzbank.queue.test --user fappricing --password fappricing
