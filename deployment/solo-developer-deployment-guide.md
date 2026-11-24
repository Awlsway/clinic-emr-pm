# Solo Developer's Guide to On-Premises Deployment

**For:** One-person EMR business  
**Audience:** You (solo developer/business owner)  
**Goal:** Deploy Clinic EMR at customer clinic without a team

---

## Reality Check: Can One Person Do This?

### ✅ **YES - It's Totally Doable!**

**Many successful software businesses started this way:**
- One developer
- On-premises installations
- Gradual growth
- Eventually hire help

**You CAN do this. Here's how.**

---

## Your Deployment Process (Solo Developer)

### Phase 1: Before You Visit the Clinic (Preparation)

**Time:** 1-2 weeks before installation

---

#### Step 1: Create Automated Installation Scripts

**Why:** You can't remember every command. Scripts make it repeatable and professional.

**What to create:**

**File: `install-clinic-emr.sh`** (Ubuntu installation script)

```bash
#!/bin/bash
# Clinic EMR Automated Installation Script
# Run this on fresh Ubuntu 22.04 LTS installation

set -e  # Exit on any error

echo "========================================="
echo "Clinic EMR Installation Script"
echo "========================================="
echo ""

# Configuration
CLINIC_NAME="$1"
ADMIN_EMAIL="$2"
POSTGRES_PASSWORD=$(openssl rand -base64 32)

if [ -z "$CLINIC_NAME" ] || [ -z "$ADMIN_EMAIL" ]; then
    echo "Usage: ./install-clinic-emr.sh \"Clinic Name\" admin@email.com"
    exit 1
fi

echo "Installing for: $CLINIC_NAME"
echo "Admin email: $ADMIN_EMAIL"
echo ""

# 1. System update
echo "Step 1/10: Updating system..."
sudo apt update
sudo apt upgrade -y

# 2. Install PostgreSQL
echo "Step 2/10: Installing PostgreSQL..."
sudo apt install -y postgresql postgresql-contrib

# 3. Install Go
echo "Step 3/10: Installing Go..."
wget https://go.dev/dl/go1.21.5.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.21.5.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
export PATH=$PATH:/usr/local/go/bin

# 4. Create database
echo "Step 4/10: Creating database..."
sudo -u postgres psql <<EOF
CREATE DATABASE clinic;
CREATE USER clinic WITH PASSWORD '$POSTGRES_PASSWORD';
GRANT ALL PRIVILEGES ON DATABASE clinic TO clinic;
\c clinic
GRANT ALL ON SCHEMA public TO clinic;
EOF

# Save password securely
echo "$POSTGRES_PASSWORD" > /opt/clinic-emr/.db-password
chmod 600 /opt/clinic-emr/.db-password

# 5. Create application directory
echo "Step 5/10: Creating application directory..."
sudo mkdir -p /opt/clinic-emr
sudo chown $USER:$USER /opt/clinic-emr

# 6. Copy application files
echo "Step 6/10: Copying application..."
# You'll copy your compiled binary and files here
cp clinic-emr /opt/clinic-emr/
cp -r templates /opt/clinic-emr/
cp -r static /opt/clinic-emr/

# 7. Create configuration
echo "Step 7/10: Creating configuration..."
cat > /opt/clinic-emr/config.env <<EOF
DATABASE_URL=postgres://clinic:$POSTGRES_PASSWORD@localhost:5432/clinic?sslmode=disable
CLINIC_NAME=$CLINIC_NAME
ADMIN_EMAIL=$ADMIN_EMAIL
PORT=8080
EOF

# 8. Set up systemd service
echo "Step 8/10: Creating system service..."
sudo tee /etc/systemd/system/clinic-emr.service > /dev/null <<EOF
[Unit]
Description=Clinic EMR Application
After=network.target postgresql.service

[Service]
Type=simple
User=$USER
WorkingDirectory=/opt/clinic-emr
EnvironmentFile=/opt/clinic-emr/config.env
ExecStart=/opt/clinic-emr/clinic-emr
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable clinic-emr
sudo systemctl start clinic-emr

# 9. Set up automatic backups
echo "Step 9/10: Setting up backups..."
sudo mkdir -p /var/backups/clinic-emr
sudo tee /usr/local/bin/backup-clinic-emr.sh > /dev/null <<'BACKUP'
#!/bin/bash
BACKUP_DIR="/var/backups/clinic-emr"
DATE=$(date +%Y%m%d-%H%M%S)
pg_dump -U clinic clinic > "$BACKUP_DIR/clinic-$DATE.sql"
# Keep last 30 days
find $BACKUP_DIR -name "clinic-*.sql" -mtime +30 -delete
BACKUP

sudo chmod +x /usr/local/bin/backup-clinic-emr.sh

# Add to crontab (daily at 2 AM)
(crontab -l 2>/dev/null; echo "0 2 * * * /usr/local/bin/backup-clinic-emr.sh") | crontab -

# 10. Set up firewall
echo "Step 10/10: Configuring firewall..."
sudo ufw allow 8080/tcp
sudo ufw allow 22/tcp  # SSH for your remote access
sudo ufw enable

echo ""
echo "========================================="
echo "Installation Complete!"
echo "========================================="
echo ""
echo "Clinic EMR is running at: http://localhost:8080"
echo "Database password saved in: /opt/clinic-emr/.db-password"
echo "Backups will run daily at 2 AM"
echo ""
echo "Next steps:"
echo "1. Test the application"
echo "2. Create admin user"
echo "3. Train clinic staff"
echo ""
```

**Save this file. You'll use it at every installation.**

---

#### Step 2: Create Installation Checklist

**File: `deployment-checklist.md`**

```markdown
# Clinic EMR Deployment Checklist

## Pre-Visit (1 week before)

- [ ] Confirm appointment date/time with clinic
- [ ] Confirm hardware ready (server/computer)
- [ ] Confirm internet connection available
- [ ] Download Ubuntu 22.04 LTS ISO
- [ ] Create bootable USB drive
- [ ] Pack equipment:
  - [ ] Laptop
  - [ ] USB drive (Ubuntu installer)
  - [ ] USB drive (application files backup)
  - [ ] Network cables (backup)
  - [ ] Mouse/keyboard (backup)
  - [ ] Power adapter
- [ ] Print user manual
- [ ] Print admin manual
- [ ] Prepare invoice/contract

## Day 1: Installation (4-6 hours)

### Morning (9 AM - 12 PM)

- [ ] Arrive at clinic
- [ ] Introduce yourself, tour facility
- [ ] Verify hardware received
- [ ] Check internet connectivity
- [ ] Backup any existing data on server (if applicable)

### Installation (10 AM - 2 PM)

- [ ] Install Ubuntu 22.04 LTS from USB
  - [ ] Create user account
  - [ ] Set strong password
  - [ ] Configure network
  - [ ] Update system

- [ ] Run installation script
  - [ ] Copy script and app files
  - [ ] Execute: `./install-clinic-emr.sh "Clinic Name" admin@email.com`
  - [ ] Verify no errors
  - [ ] Check application running

### Lunch Break (12 PM - 1 PM)

- [ ] Eat lunch (application installing)

### Afternoon (1 PM - 5 PM)

- [ ] Test application
  - [ ] Access http://localhost:8080
  - [ ] Create admin user
  - [ ] Create test patient
  - [ ] Test search
  - [ ] Test edit
  - [ ] Test FHIR export
  - [ ] Verify backups configured

- [ ] Configure remote access for you
  - [ ] Set up SSH access
  - [ ] Configure firewall rules
  - [ ] Test remote connection
  - [ ] Save connection details

- [ ] Security hardening
  - [ ] Change default passwords
  - [ ] Configure firewall
  - [ ] Set up fail2ban (block brute force)
  - [ ] Enable automatic security updates

## Day 2: Training and Go-Live (4-6 hours)

### Morning (9 AM - 12 PM): Training

- [ ] Train receptionist (patient registration)
- [ ] Train nurses (patient search/edit)
- [ ] Train doctors (patient records, FHIR export)
- [ ] Train admin (user management, backups)

### Afternoon (12 PM - 5 PM): Go-Live

- [ ] Import initial patient data (if migrating)
- [ ] Run through real workflows
- [ ] Address questions
- [ ] Document any customization requests
- [ ] Verify everything working

### Wrap-Up

- [ ] Give admin manual to clinic manager
- [ ] Give user manual to staff
- [ ] Provide support contact info (your phone/email)
- [ ] Collect payment/sign contract
- [ ] Schedule follow-up call (1 week)
- [ ] Take photos (for documentation)

## Post-Installation (First Week)

- [ ] Day 1: Call clinic to check in
- [ ] Day 3: Remote check (logs, backups)
- [ ] Day 7: Follow-up call, address issues
- [ ] Update deployment documentation with lessons learned
```

---

#### Step 3: Prepare Your Equipment

**What to bring to clinic:**

**Required:**
1. ✅ Your laptop (for remote configuration)
2. ✅ USB drive with Ubuntu 22.04 LTS installer
3. ✅ USB drive with your application files (backup copy)
4. ✅ Printed user manual
5. ✅ Printed admin manual
6. ✅ Contract/invoice

**Optional but helpful:**
1. Network cable (in case WiFi doesn't work)
2. Extra mouse/keyboard (in case theirs breaks)
3. USB hub
4. External hard drive (for large data migrations)

---

### Phase 2: At the Clinic (Installation Day)

**Timeline:** 2 days on-site

---

#### Day 1: Technical Setup (6 hours)

**9:00 AM - Arrival**
- Introduce yourself professionally
- Tour the facility
- Meet key staff (manager, head nurse, IT person if any)
- Find where server will be located
- Check internet connection

**9:30 AM - Hardware Check**
- Unbox server (if new)
- Connect power, network, monitor, keyboard, mouse
- Power on and verify it works
- Check BIOS settings (boot from USB enabled)

**10:00 AM - Install Ubuntu**
1. Insert Ubuntu USB drive
2. Reboot, press F12 (or F2/Del for boot menu)
3. Select USB drive
4. Follow Ubuntu installer:
   - Select "Install Ubuntu"
   - Language: English
   - Keyboard: US
   - Installation type: "Erase disk and install Ubuntu" (use whole disk)
   - Location: Select timezone
   - Create account:
     - Name: clinic-admin (or clinic name)
     - Computer name: clinic-emr-server
     - Password: **Strong password** (write down, give to clinic manager)
5. Wait for installation (~20 minutes)
6. Reboot
7. Remove USB drive
8. Log in

**11:00 AM - Run Installation Script**

```bash
# Update system first
sudo apt update && sudo apt upgrade -y

# Copy your installation script and app files from USB
cp /media/usb/install-clinic-emr.sh ~/
cp /media/usb/clinic-emr ~/
cp -r /media/usb/templates ~/
cp -r /media/usb/static ~/

# Make script executable
chmod +x install-clinic-emr.sh

# Run installation
./install-clinic-emr.sh "Happy Clinic" admin@happyclinic.com
```

**12:00 PM - Lunch Break**
- Let installation finish while you eat

**1:00 PM - Testing**

Test every feature systematically:

```bash
# 1. Check application running
sudo systemctl status clinic-emr
# Should show "active (running)"

# 2. Check database
sudo -u postgres psql -d clinic -c "SELECT version();"
# Should show PostgreSQL version

# 3. Access web application
firefox http://localhost:8080
# Should load application homepage
```

From the web interface:
1. Create admin user
2. Register test patient
3. Search for patient
4. Edit patient
5. Deactivate/reactivate patient
6. Export FHIR
7. Test all features from your UAT

**3:00 PM - Configure Remote Access**

This lets you support the clinic remotely:

```bash
# 1. Get clinic's public IP address
curl ifconfig.me

# 2. Enable SSH (secure remote access)
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh

# 3. Configure firewall
sudo ufw allow 22/tcp  # SSH
sudo ufw allow 8080/tcp  # Clinic EMR
sudo ufw enable

# 4. Test SSH from your laptop
ssh clinic-admin@192.168.x.x  # Local IP
# Later test from your office with public IP
```

**4:00 PM - Security Hardening**

```bash
# 1. Change default PostgreSQL password
sudo -u postgres psql
ALTER USER postgres WITH PASSWORD 'new-secure-password';

# 2. Install fail2ban (blocks brute force attacks)
sudo apt install fail2ban
sudo systemctl enable fail2ban

# 3. Enable automatic security updates
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
# Select "Yes"

# 4. Set up monitoring (optional but recommended)
# Install basic monitoring
sudo apt install htop iotop
```

**5:00 PM - Document Everything**

Before you leave, write down:
- Server username/password (give to clinic manager, sealed envelope)
- Database password location (`/opt/clinic-emr/.db-password`)
- Application URL (http://localhost:8080)
- Your contact information
- Backup location (`/var/backups/clinic-emr`)
- Any issues encountered and how you fixed them

**End of Day 1** - Technical setup complete!

---

#### Day 2: Training and Go-Live (6 hours)

**9:00 AM - Quick System Check**
- Verify application still running after overnight
- Check backups ran successfully
- Review any overnight logs

**9:30 AM - Training Session 1: Basic Users**

**Train receptionists (30 minutes):**
- How to register new patient
- How to search for patient
- How to update contact information
- Demo: Register 2-3 real patients together

**10:00 AM - Training Session 2: Clinical Staff**

**Train nurses/doctors (45 minutes):**
- How to search patients
- How to update medical information
- How to export FHIR (if needed)
- How to deactivate patients
- Demo: Work through actual workflows

**11:00 AM - Training Session 3: Admin**

**Train clinic manager (30 minutes):**
- How to check system status
- How to verify backups
- Where to find documentation
- How to contact you for support
- Emergency procedures

**12:00 PM - Lunch Break**

**1:00 PM - Supervised Practice**

Let staff use the system while you watch:
- They do their actual work
- You observe and help
- Answer questions
- Note any confusion or issues

**3:00 PM - Go-Live Decision**

Ask: "Are you comfortable using this for real patients?"

If YES:
- Remove any test patients
- They start using for real
- You stay available

If NO:
- More practice time
- Address concerns
- Reschedule go-live

**4:00 PM - Paperwork and Wrap-Up**

- Sign service contract
- Collect payment (or send invoice)
- Give them:
  - User manual (printed)
  - Admin manual (printed)
  - Your contact card
  - Server credentials (sealed envelope)
- Schedule follow-up call (next week)
- Take photos for your records

**5:00 PM - Depart**

Thank everyone, remind them you're available for support.

---

### Phase 3: Post-Installation (Remote Support)

**You can do this from your home/office!**

---

#### Week 1: Daily Check-Ins

**Monday (Day after installation):**
```bash
# SSH into clinic server
ssh clinic-admin@clinic-public-ip

# Check application status
sudo systemctl status clinic-emr

# Check logs for errors
sudo journalctl -u clinic-emr -n 100

# Check disk space
df -h

# Check memory usage
free -h

# Call clinic: "How's it going? Any questions?"
```

**Tuesday-Friday:**
- Quick remote check (5 minutes/day)
- Answer any support emails/calls

---

#### Week 2-4: Monitoring

**Weekly tasks (30 minutes):**

```bash
# SSH to clinic
ssh clinic-admin@clinic-public-ip

# Verify backups running
ls -lh /var/backups/clinic-emr/
# Should see daily backup files

# Check logs
sudo journalctl -u clinic-emr --since "1 week ago" | grep ERROR

# Check system updates
sudo apt update
sudo apt list --upgradable

# Apply updates if safe
sudo apt upgrade -y
```

**Monthly call with clinic:**
- "How is everything going?"
- "Any features you'd like to see?"
- "Any issues or slowness?"

---

### Phase 4: Ongoing Support (As Needed)

**Remote Support (90% of issues):**

Most issues you can fix remotely:

```bash
# Application not responding?
sudo systemctl restart clinic-emr

# Database slow?
sudo -u postgres psql -d clinic -c "VACUUM ANALYZE;"

# Disk filling up?
# Clean old backups
sudo find /var/backups/clinic-emr -name "*.sql" -mtime +30 -delete

# Update application
# (upload new binary via scp)
scp clinic-emr clinic-admin@clinic-ip:/opt/clinic-emr/
ssh clinic-admin@clinic-ip "sudo systemctl restart clinic-emr"
```

**On-Site Support (10% of issues):**

Only need to visit for:
- Hardware failures
- Network issues you can't debug remotely
- Major upgrades
- Quarterly check-ups (optional)

---

## Solo Developer Pro Tips

### 1. **Automate Everything You Can**

**Create scripts for common tasks:**

**File: `remote-check.sh`** (Run from your office)

```bash
#!/bin/bash
# Quick remote health check

CLINIC_IP="$1"

if [ -z "$CLINIC_IP" ]; then
    echo "Usage: ./remote-check.sh clinic-ip-address"
    exit 1
fi

echo "Checking clinic at $CLINIC_IP..."
echo ""

# Check if server is up
if ping -c 1 $CLINIC_IP &> /dev/null; then
    echo "✓ Server is online"
else
    echo "✗ Server is OFFLINE!"
    exit 1
fi

# Check application
APP_STATUS=$(ssh clinic-admin@$CLINIC_IP "sudo systemctl is-active clinic-emr")
if [ "$APP_STATUS" == "active" ]; then
    echo "✓ Application is running"
else
    echo "✗ Application is NOT running!"
fi

# Check last backup
LAST_BACKUP=$(ssh clinic-admin@$CLINIC_IP "ls -t /var/backups/clinic-emr/*.sql | head -1")
echo "✓ Last backup: $LAST_BACKUP"

# Check disk space
DISK_USAGE=$(ssh clinic-admin@$CLINIC_IP "df -h / | tail -1 | awk '{print \$5}'")
echo "✓ Disk usage: $DISK_USAGE"

echo ""
echo "Health check complete!"
```

Run this weekly:
```bash
./remote-check.sh 123.45.67.89  # Clinic's public IP
```

---

### 2. **Document Everything**

**Keep a deployment log for each clinic:**

```markdown
# Clinic: Happy Clinic
# Installed: 2026-11-20
# Server IP: 192.168.1.50 (local), 123.45.67.89 (public)

## Installation Notes:
- Server: HP ProDesk 400 G6
- RAM: 16 GB
- Disk: 512 GB SSD
- Ubuntu: 22.04 LTS
- PostgreSQL: 14.x
- Issues encountered: None
- Special requests: None

## Contacts:
- Manager: Dr. Smith, 555-1234, smith@happyclinic.com
- IT contact: None

## Access:
- SSH: clinic-admin@123.45.67.89 (password in password manager)
- Database: postgres://clinic:PASSWORD@localhost/clinic
- Application: http://localhost:8080

## Backup Schedule:
- Daily at 2 AM
- Location: /var/backups/clinic-emr/
- Retention: 30 days

## Support History:
- 2026-11-25: Called to ask about search, explained
- 2026-12-01: Remote restart, slow performance (VACUUM ran)
```

---

### 3. **Set Up Monitoring Alerts**

**Use simple monitoring tools:**

**Option A: UptimeRobot** (Free)
- Website: uptimerobot.com
- Set up HTTP monitor for http://clinic-ip:8080
- Get email if application goes down

**Option B: Cron + Email**

On clinic server:
```bash
# Create monitoring script
cat > /usr/local/bin/health-check.sh <<'EOF'
#!/bin/bash
if ! systemctl is-active --quiet clinic-emr; then
    echo "Clinic EMR is DOWN!" | mail -s "ALERT: Clinic EMR Down" your@email.com
fi
EOF

chmod +x /usr/local/bin/health-check.sh

# Run every hour
(crontab -l; echo "0 * * * * /usr/local/bin/health-check.sh") | crontab -
```

---

### 4. **Charge Appropriately**

**Sample Pricing (Solo Developer):**

**On-Premises Installation:**
- Installation fee: $2,000-3,000 (covers your 2 days + travel)
- Annual support: $1,200 ($100/month)
  - Remote support (email/phone)
  - Software updates
  - Backup monitoring
- On-site visits: $500/day + travel (if needed)

**What's included in support:**
- Remote troubleshooting
- Software updates
- Backup verification
- Email/phone support (business hours)

**What's extra:**
- On-site visits (except emergencies)
- Customizations
- Data migrations
- Training additional staff

---

### 5. **Manage Your Time**

**As solo developer, you can realistically handle:**

**Current capacity:**
- 2-3 installations per month (6 days/month)
- 5-10 clinics total (ongoing support)
- 10-20 hours/month remote support

**When to hire help:**
- 10+ active clinics
- Support calls exceeding 20 hours/month
- Can't take vacation without worry

---

## Common Issues and Solutions

### Issue 1: "Application won't start"

```bash
# Check logs
sudo journalctl -u clinic-emr -n 50

# Common causes:
# - Database not running
sudo systemctl status postgresql
sudo systemctl start postgresql

# - Port already in use
sudo lsof -i :8080

# - Configuration error
cat /opt/clinic-emr/config.env
```

---

### Issue 2: "Can't connect remotely"

```bash
# On clinic server, check SSH running
sudo systemctl status ssh

# Check firewall
sudo ufw status

# Verify clinic's public IP hasn't changed
curl ifconfig.me

# Try from your laptop
ssh -v clinic-admin@clinic-ip
# -v shows verbose debugging
```

---

### Issue 3: "Backup failed"

```bash
# Check backup script
cat /usr/local/bin/backup-clinic-emr.sh

# Run manually to see errors
sudo /usr/local/bin/backup-clinic-emr.sh

# Check disk space
df -h /var/backups/clinic-emr/

# Check cron logs
grep backup /var/log/syslog
```

---

### Issue 4: "System slow"

```bash
# Check disk usage
df -h

# Check memory
free -h

# Check CPU
htop

# Vacuum database
sudo -u postgres psql -d clinic -c "VACUUM ANALYZE;"

# Restart application
sudo systemctl restart clinic-emr
```

---

## Your First Installation Timeline

**3 Weeks Before:**
- Create all scripts and documentation
- Test installation on VM
- Find first customer

**1 Week Before:**
- Confirm installation date
- Prepare equipment
- Create Ubuntu USB
- Print manuals

**Installation (2 days):**
- Day 1: Technical setup (6 hours)
- Day 2: Training and go-live (6 hours)

**First Week:**
- Daily check-ins (remote)
- Available for support

**First Month:**
- Weekly check-ins
- Build confidence

**Ongoing:**
- Monthly monitoring
- Updates as needed
- Support as needed

---

## Can You Really Do This Alone?

### ✅ **Absolutely YES!**

**You already have the skills:**
- ✅ You built the application
- ✅ You completed UAT testing
- ✅ You understand the system

**What you're adding:**
- Installing Ubuntu (straightforward)
- Running scripts (you can write these)
- Training users (just explaining your app)
- Remote support (SSH + system commands)

**Thousands of solo developers do this successfully.**

---

## Your Biggest Advantages as Solo Developer

1. **Flexibility:** You control your schedule
2. **Direct relationship:** Customers deal with YOU (the creator)
3. **Quick decisions:** No bureaucracy
4. **Learn fast:** Every installation teaches you
5. **Keep all revenue:** No team to pay

---

## Final Checklist: Are You Ready?

- [ ] Application works perfectly (UAT passed)
- [ ] Installation script created
- [ ] Documentation written (user + admin manuals)
- [ ] Equipment prepared (laptop, USB drives)
- [ ] Pricing decided
- [ ] Contract/invoice template ready
- [ ] Remote access plan (SSH, monitoring)
- [ ] Support procedures documented
- [ ] First customer identified
- [ ] Appointment scheduled

---

## Next Steps

1. **Create the installation script** (copy from this guide)
2. **Test on a VM** (VirtualBox + Ubuntu)
3. **Practice the installation** (time yourself)
4. **Find your first customer** (friend's clinic?)
5. **Do the installation** (2 days on-site)
6. **Learn and improve** (document lessons)
7. **Repeat** (get better each time)

---

**You've got this!** 🚀

Every successful software business started with one person doing their first installation.

**Questions? Concerns? Let's discuss!**

---

**END OF GUIDE**
