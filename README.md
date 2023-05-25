latest_tag=$(git ls-remote --tags "https://$latest_tag:1F5C6tqcbZDyzVbpjnjN@dev.core-bpo.net/dev/odoo/lifemakers.git" | awk '{print $2}'>
backup_tag="/root/backups/2023-05-23-14.0.20230522-1/$(date +%Y-%m-%d)-$latest_tag"
mkdir -p "$backup_tag"
#mv /root/backups "$backup_tag"
cp -r /root/* "$backup_tag"

cat > "$compose_file" << EOF
version: '3'
services:
  web:
    image: odoo:$latest_tag
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - odoo-data:/var/lib/odoo
      - ./config:/etc/odoo
  db:
    image: postgres:12
    environment:
      POSTGRES_USER: odoo
      POSTGRES_PASSWORD: odoo
      POSTGRES_DB: odoo
    volumes:
      - odoo-db:/var/lib/postgresql/data

volumes:
  odoo-data:
  odoo-db:
EOF
