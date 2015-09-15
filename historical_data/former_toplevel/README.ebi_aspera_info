If you wish to use aspera with the EBI server you need to first download the client 
from aspera

http://downloads.asperasoft.com/download_connect/

This will provide you with both the web plugin and the ascp command line tool

If you visit http://www.1000genomes.org/aspera with your web browser after installing
aspera connect it should automatically launch the plugin and you should be ready to go.

For the command line tool you need to use a command line like

ascp -i bin/aspera/etc/asperaweb_id_dsa.putty -Tr -Q -l 100M -L- fasp-g1k@fasp.1000genomes.ebi.ac.uk:vol1/ftp/release/20100804/ALL.2of4intersection.20100804.genotypes.vcf.gz ./

For this to work with your network's firewall you need to:

Please open ports:
    22/tcp (outgoing) and
    33001/udp (both incoming and outgoing)

to the following EBI IPs:
    193.62.192.6
    193.62.193.6

If the firewall has UDP flood protection, it must be turned off for port 33001.

