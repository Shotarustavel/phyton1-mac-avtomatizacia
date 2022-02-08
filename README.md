# phyton1-mac-avtomatizacia
phyton 1 mac atomatizacia
import subprocess
import optparse
import re

def get_arguments():
    parser = optparse.OptionParser()
    parser.add_option("-i", "--interface", dest="interface", help="Interface to change MAQ address")
    parser.add_option("-e", "--eth0", dest="new_eth0", help="new MAC Address")
    (options, arguments) = parser.parse_args()
    if not options.interface:
        parser.error("[-] please specify an interface, use --help for more info")
    elif not options.new_eth0:
        parser.error("[-] Please specify a new mac, use --help for more info.")
    return options
def change_mac(interface, new_eth0):
    print("[+] Changing MAC address for " + interface + " to " + new_eth0)
    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_eth0])
    subprocess.call(["ifconfig", interface, "up"])
options = get_arguments()
change_mac(options.interface, options.new_eth0)
ifconfig_result = subprocess.check_output(["ifconfig", options.interface])
print(ifconfig_result)
mac_adrdress_search_result = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w",ifconfig_result)
if mac_adrdress_search_result:
    print(mac_adrdress_search_result.group(0))
else:
    print("[-] Could not read MAC addres")
