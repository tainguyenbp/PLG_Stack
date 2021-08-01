# /home/nginx/domains/*/log/access.log
# Nginx 444  (Default: 5 errors bans for 24 hours)
if (($globlogs{CUSTOM1_LOG}{$lgfile}) and ($line =~ /(\S+) -.*[GET|POST|HEAD].*(444)/)) {
    return ("Nginx 444",$1,"nginx_444","5","80,443","86400","0");
}

# /var/log/nginx/localhost.access.log
# Nginx 444  (Default: 5 errors bans for 24 hours)
if (($globlogs{CUSTOM3_LOG}{$lgfile}) and ($line =~ /(\S+) -.*[GET|POST|HEAD].*(444)/)) {
    return ("Nginx 444",$1,"nginx_444","5","80,443","86400","0");
}

# /home/nginx/domains/*/log/error.log
# NginX security rules trigger (Default: 40 errors bans for 24 hours)
if (($globlogs{CUSTOM2_LOG}{$lgfile}) and ($line =~ /.*access forbidden by rule, client: (\S+).*/)) {
    return ("NGINX Security rule triggered from",$1,"nginx_security","40","80,443","86400","0");
}

# /var/log/nginx/localhost.error.log
# NginX security rules trigger (Default: 40 errors bans for 24 hours)
if (($globlogs{CUSTOM4_LOG}{$lgfile}) and ($line =~ /.*access forbidden by rule, client: (\S+).*/)) {
    return ("NGINX Security rule triggered from",$1,"nginx_security","40","80,443","86400","0");
}

# /home/nginx/domains/*/log/error.log
# NginX 404 errors (Default: 50 errors bans for 24 hours)
if (($globlogs{CUSTOM2_LOG}{$lgfile}) and ($line =~ /.*No such file or directory\), client: (\S+),.*/)) {
    return ("NGINX Security rule triggered from",$1,"nginx_404s","50","80,443","86400","0");
}

# /var/log/nginx/localhost.error.log
# NginX 404 errors (Default: 50 errors bans for 24 hours)
if (($globlogs{CUSTOM4_LOG}{$lgfile}) and ($line =~ /.*No such file or directory\), client: (\S+),.*/)) {
    return ("NGINX Security rule triggered from",$1,"nginx_404s","50","80,443","86400","0");
}

# /home/nginx/domains/*/log/access.log
#Trying to download htaccess or htpasswd  (Default: 1 error bans for 24 hours)
if (($globlogs{CUSTOM1_LOG}{$lgfile}) and ($line =~ /.*\.(htpasswd|htaccess).*client: (\S+),.*GET/)) {
    return ("Trying to download .ht files",$1,"nginx_htfiles","1","80,443","86400","0");
}

# /var/log/nginx/localhost.access.log
#Trying to download htaccess or htpasswd  (Default: 1 error bans for 24 hours)
if (($globlogs{CUSTOM3_LOG}{$lgfile}) and ($line =~ /.*\.(htpasswd|htaccess).*client: (\S+),.*GET/)) {
    return ("Trying to download .ht files",$1,"nginx_htfiles","1","80,443","86400","0");
}

# Wordpress fail2ban plugin https://wordpress.org/plugins/wp-fail2ban-redux/
# (Default: 2 errors bans for 24 hours)
if (($globlogs{SYSLOG_LOG}{$lgfile}) and ($line =~ /.*Authentication attempt for unknown user .* from (.*)\n/)) {
  return ("Wordpress unknown user from",$1,"fail2ban_unknownuser","2","80,443","86400","0");
}

# Wordpress fail2ban plugin https://wordpress.org/plugins/wp-fail2ban-redux/
# (Default: 2 errors bans for 24 hours)
if (($globlogs{SYSLOG_LOG}{$lgfile}) and ($line =~ /.*Blocked user enumeration attempt from (.*)\n/)) {
  return ("WordPress user enumeration attempt from",$1,"fail2ban_userenum","2","80,443","86400","0");
}

# Wordpress fail2ban plugin https://wordpress.org/plugins/wp-fail2ban-redux/
# (Default: 2 errors bans for 24 hours)
if (($globlogs{SYSLOG_LOG}{$lgfile}) and ($line =~ /.*Pingback error .* generated from (.*)\n/)) {
  return ("WordPress pingback error",$1,"fail2ban_pingback","2","80,443","86400","0");
}

# Wordpress fail2ban plugin https://wordpress.org/plugins/wp-fail2ban-redux/
# (Default: 2 errors bans for 24 hours)
if (($globlogs{SYSLOG_LOG}{$lgfile}) and ($line =~ /.*Spammed comment from (.*)\n/)) {
  return ("WordPress spam comments from",$1,"fail2ban_spam","2","80,443","86400","0");
}

# Wordpress fail2ban plugin https://wordpress.org/plugins/wp-fail2ban-redux/
# (Default: 2 errors bans for 24 hours)
if (($globlogs{SYSLOG_LOG}{$lgfile}) and ($line =~ /.*XML-RPC multicall authentication failure (.*)\n/)) {
  return ("WordPress XML-RPC multicall fail from",$1,"fail2ban_xmlrpc","5","80,443","86400","0");
}

# /home/nginx/domains/*/log/error.log
# https://community.centminmod.com/posts/74546/
# Nginx connection limit rule trigger (Default: 30 errors bans for 60mins)
if (($globlogs{CUSTOM2_LOG}{$lgfile}) and ($line =~ /.*limiting connections by zone .*, client: (\S+),(.*)/)) {
    return ("NGINX Security rule triggered from",$1,"nginx_conn_limit","30","80,443","3600","0");
}

# /var/log/nginx/localhost.error.log
# https://community.centminmod.com/posts/74546/
# Nginx connection limit rule trigger (Default: 30 errors bans for 60mins)
if (($globlogs{CUSTOM4_LOG}{$lgfile}) and ($line =~ /.*limiting connections by zone .*, client: (\S+),(.*)/)) {
    return ("NGINX Security rule triggered from",$1,"nginx_conn_limit_localhost","30","80,443","3600","0");
}
