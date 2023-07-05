# Installation

Download and install the public signing key:

    wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

Installing from the APT repository:

    sudo apt-get install apt-transport-https

Save the repository definition to /etc/apt/sources.list.d/elastic-8.x.list:

    echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

Install the Elasticsearch Debian package with:

    sudo apt-get update && sudo apt-get install elasticsearch

If any error:

    # Enable security features
    xpack.security.enabled: false