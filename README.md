# wg-peer-update

This script allows to automatically update the public IP router address in a site-to-site Wireguard VPN when both routers have a dynamic public IP address and one of the two changes (tipically after a reboot).<br/>
It can be used by creating a netwatch on both routers...<br/>
<br/>
Below the explenation:<br/>
<br/>
<b>xxxxxxxx.sn.mynetname.net</b> is the hostname of the remote router (You can enable it on /ip/cloud/ddns...);<br/>
<b>wgX</b> is the name of the wireguard tunnel interface, generally it's called wg0, in this example it's also the interface comment;<br/>
<b>wgXold</b> and <b>wgXnew</b> it's the respective variable with the old and new ip address;<br/>
<b>x.x.x.x</b> is the remote wireguard IP address, for example 172.17.0.2;<br/>

<pre>
  <code>
/tool/netwatch
add down-script=":global wgXnew [:resolve xxxxxxxx.sn.mynetname.net]\
    \n#:global wgXold [/interface wireguard peers get [find comment=\"wgX\"] endpoint-address ]\
    \n:global wgXold [/interface wireguard peers get value-name=endpoint-address [find interface=\"wgX\"]]\
    \n\
    \n:if (\$wgXold != \$wgXnew) do={\
    \n/interface wireguard peers set endpoint-address=\"\$wgXnew\" [find interface=\"wgX\"]\
    \n/log info \"Wireguard wgX ip updated\";\
    \n}" host=x.x.x.x interval=2m
  </code>
</pre>
