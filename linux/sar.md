# System activity report

- https://bencane.com/2012/07/08/sar-sysstat-linux-performance-statistics-with-ease/
- https://www.crybit.com/sysstat-sar-on-ubuntu-debian/
- http://www.leonardoborda.com/blog/how-to-configure-sysstatsar-on-ubuntudebian/

 ```bash 
sed -i 's/ENABLED="false"/ENABLED="true"/' /etc/default/sysstat
sed -i 's/5-55\/10/*/' /etc/cron.d/sysstat
service sysstat restart
 ```