
# All Nodes  

## Configure Metrics Export Tools 
### Summary
    (At time of writing) sets up:
    - Node Exporter w/ filesystem collector for a reasonable set of sources (many more collectors are available https://github.com/prometheus/node_exporter)
    - cAdvisor for CPU, memory, file io and network stats

### CLI commands
#### Perform basic set-up on all servers (install apt, docker, etc.)
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook 
-u root -i '${element(digitalocean_droplet.prometheus.*.ipv4_address, count.index)},'  
--private-key ${var.do_pvt_key} -e 'pub_key=${var.do_pub_key}' 
ansible/playbooks/apt_docker.yml"
#####
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -u root -i '192.168.0.157'  --private-key "/home/penguaman/.ssh/id_rsa" -e 'pub_key="/home/penguaman/.ssh/id_rsa"' ansible/playbooks/apt_docker.yml


#### Configure Node as target 
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook \ 
-u root -i '${element(digitalocean_droplet.archiveteam.*.ipv4_address, count.index)},' \
--private-key ${var.do_pvt_key} -e 'pub_key=${var.do_pub_key}' \
 ansible/playbooks/target_nodes.yml"

##### Options Details
1. **`ANSIBLE_HOST_KEY_CHECKING=False`**  
   Disables SSH host key checking in Ansible, allowing playbook execution without manual confirmation of host keys.

2. **`ansible-playbook`**  
   The Ansible command used to execute playbooks that define sets of automation tasks for one or more hosts.

3. **`-u root`**  
   Specifies the remote user as `root` for performing the configuration tasks.

4. **`-i '${element(digitalocean_droplet.archiveteam.*.ipv4_address, count.index)},'`**
   The `element` function w/ count.index selects the specific IP for the current 
   server. Whole command runs once for each item in this list 

5. **`--private-key ${var.do_pvt_key}`**  
   Points to the SSH private key file to authenticate the Ansible SSH connection.

6. **`-e 'pub_key=${var.do_pub_key}'`**  
   Passes an extra variable (`pub_key`) to Ansible at runtime. Within the ansible 
   playbooks, the string `${var.do_pub_key}` is replaced with the passed value.

7. **`ansible/playbooks/target_nodes.yml`**  
   The specific Ansible playbook file that contains the list of tasks and roles to be applied to the target node in `4.`.


#### Run Observer specific setup 
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook \
-u root -i '${element(digitalocean_droplet.prometheus.*.ipv4_address, count.index)},'  \
--private-key ${var.do_pvt_key} \
-e 'nodes=${"${jsonencode(formatlist("%s:9100", digitalocean_droplet.archiveteam.*.ipv4_address))}"} prom_ip=${"${jsonencode(digitalocean_droplet.prometheus.*.ipv4_address[0])}"} \
ca_nodes=${"${jsonencode(formatlist("%s:9101", digitalocean_droplet.archiveteam.*.ipv4_address))}"}' \
ansible/playbooks/observer.yml
