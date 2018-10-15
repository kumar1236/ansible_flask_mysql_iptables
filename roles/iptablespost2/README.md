# iptablespost2
* Based off original (iptablespost)
* writes content to /root/iptables/ and copies to hosts, instead of dynamically generating for each host
* iptables tag still valid and functions as it did previously

### Prerequisites:
* group_vars entries defining filename to be created
  * example:  group_vars/all_oxapp:iptables_group: oxapp.{{config_environment}}
  * config_environment comes from inventory (e.g. production, qa_openstack)
  * above generates /root/iptables/ip4tables.oxapp.production and /root/iptables/ip6tables.oxapp.production for production

* playbooks remain largely unchanged, but host groups **MUST** be broken out with one group per iptablespost2 task
  * this is due to the run_once directive, so it's not constantly creating the same iptables rules for each host
  * iptables pre can still remain grouped

### Misc:
* the first time this runs it will always show a change, due to the file not being in /root/iptables
* there is always one host (the first in the iteration) that shows as a change when creating the file (vs all in a group)
  * this same host will always show "extra" changes beyond the others due to the file creation/update being associated with it
  * after the initial file is created, changes to be made will be shown as expected (based on that file)
* adding "strategy: free" to iptablespre allows fact collection to run in parallel (default being linear)

