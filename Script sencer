# ======================================================
# Script de Automatització per a MikroTik (RouterOS v7+)
# Adaptat a la configuració actual d'interfícies:
# - ether1: WAN (dinàmica)
# - ether2: LAN (192.168.5.1/24)
# ======================================================

# --------------------------
# 1. Configuració Bàsica
# --------------------------
/system identity set name="Router-Office"
/ip service disable telnet,ftp,www-ssl,api,api-ssl

# Configurar DNS i NTP
/ip dns set servers=8.8.8.8,1.1.1.1 allow-remote-requests=yes
/system ntp client set enabled=yes primary-ntp=pool.ntp.org

# Configurar huso horari (Europa/Madrid)
/system clock set time-zone-name="Europe/Madrid"

# --------------------------
# 2. Configuració de Xarxa
# --------------------------
# Configurar DHCP Server a la interfície LAN (ether2)
/ip pool add name=dhcp_pool ranges=192.168.5.100-192.168.5.200
/ip dhcp-server add interface=ether2 address-pool=dhcp_pool disabled=no name=dhcp1
/ip dhcp-server network add address=192.168.5.0/24 gateway=192.168.5.1 dns-server=8.8.8.8

# --------------------------
# 3. Seguretat Bàsica
# --------------------------
# Regles de firewall per a LAN (ether2) i WAN (ether1)
/ip firewall filter add chain=input action=drop connection-state=invalid comment="Bloquear conexiones inválidas"
/ip firewall filter add chain=input action=accept protocol=icmp comment="Permitir ICMP (ping)"
/ip firewall filter add chain=input action=accept connection-state=established,related
/ip firewall filter add chain=input action=drop in-interface=ether1 comment="Bloquear tráfico no autorizado desde WAN"

# Restringir serveis a la LAN (ether2)
/ip service set ssh address=192.168.5.0/24
/ip service set winbox address=192.168.5.0/24
/tool mac-server set allowed-interface-list=LAN

# --------------------------
# 4. Automatització de Backups
# --------------------------
/system scheduler add name="Backup-Diario" interval=1d on-event="/system backup save name=backup-[/system clock get date]" start-time=02:00:00

# --------------------------
# 5. Actualitzacions Automàtiques
# --------------------------
/system scheduler add name="Actualizar" interval=15d on-event="/system package update check-for-updates; delay 10; /system package update install" start-time=04:00:00

# --------------------------
# 6. Gestió d'Usuaris
# --------------------------
/user add name=adminadmin group=full password="contraseñaSegura123" 
/user remove admin

# ======================================================
# FI del Script
# ======================================================
