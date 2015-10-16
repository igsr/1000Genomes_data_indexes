#Using Aspera at EBI

If you wish to use aspera with the EBI server you need to first download the client from aspera:

http://downloads.asperasoft.com/download_connect/

This will provide you with both the web plugin and the ascp command line tool.

If you visit http://www.1000genomes.org/aspera with your web browser after installing
aspera connect, it should automatically launch the plugin and you should be ready to go.

For the command line tool ascp, for versions 3.3.3 and newer, you need to use a command line like:

         ascp -i bin/aspera/etc/asperaweb_id_dsa.openssh -Tr -Q -l 100M -L- fasp-g1k@fasp.1000genomes.ebi.ac.uk:vol1/ftp/release/20100804/ALL.2of4intersection.20100804.genotypes.vcf.gz ./

For versions 3.3.2 and older, you need to use a command line like:

         ascp -i bin/aspera/etc/asperaweb_id_dsa.putty -Tr -Q -l 100M -L- fasp-g1k@fasp.1000genomes.ebi.ac.uk:vol1/ftp/release/20100804/ALL.2of4intersection.20100804.genotypes.vcf.gz ./

Note, the only change between these commands is that for newer version fo ascp asperaweb_id_dsa.openssh replaces asperaweb_id_dsa.putty. This change is noted by Aspera [here](https://support.asperasoft.com/entries/38675468-Command-line-ascp-transfer-asking-for-a-passphrase-after-Connect-plugin-upgrade). You can check the version of ascp you have using:

       ascp --version

For this to work with your network's firewall you need to open ports 22/tcp (outgoing) and 33001/udp (both incoming and outgoing) to the following EBI IPs:

- 193.62.192.6
- 193.62.193.6

If the firewall has UDP flood protection, it must be turned off for port 33001.

For further information, please contact info@1000genomes.org.
