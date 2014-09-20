description "Mailcatcher"

start on startup
stop on shutdown
respawn

pre-start script

bash << "EOF"
  mkdir -p /var/log/mailcatcher
  chown -R vagrant /var/log/mailcatcher
EOF

end script

exec su - vagrant -c 'mailcatcher --http-ip=0.0.0.0 &>>/var/log/mailcatcher/mailcatcher.log'
