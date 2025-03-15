CIS bench marking tools check server against vulnerabilities.
CIS = center for internet security.
website = www.cisecurity.org/cis-benchmarks/
we can use "cis-cat lite" as well. 

# Demo - 
1. tool - CIS-CAT Pro Assessor tool called Assessor-CLI, under /root.
./Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index .
- /Assessor-CLI.sh → Runs the script (probably a security or assessment tool).
- -i → Likely an "interactive" or "initiate" mode.
- -rd /var/www/html/ → Sets the root directory for scanning (/var/www/html/ suggests it’s a web application).
- -nts → Could mean "no timestamp" or something similar (depends on the tool).
- -rp index → Likely specifies "report prefix", naming the report as index.

###### ######## CIS for Kuberetes ###### #############
1. CIS-CAT lite = window10, ubuntu, google crome, macos, download - https://learn.cisecurity.org/cis-cat-lite .
2. CIS-CAT pro = kubernetes, more benchmarks, download - https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro .

# KUBE-BENCH tool (from aqua security).

# Installation - 
curl -L -o kube-bench.tar.gz https://github.com/aquasecurity/kube-bench/releases/download/v0.4.0/kube-bench_0.4.0_linux_amd64.tar.gz .
file = kube-bench_0.4.0_linux_amd64.tar.gz .
website = https://github.com/aquasecurity/kube-bench/releases/tag/v0.4.0 .
1. then you get - kube-bench.tar.gz  .
2. untar - tar -xvzf kube-bench.tar.gz .
3. ./kube-bench

# k8s security - 
1. first line of defence is controlling kube-apiserver and for this we need 2 things - 
    - who can access the cluster. (userid, password, certs, ldap, service accounts).
    - what can they do. (RBAC, ABAC, node auth, webhooks).
