# WCA Competition Email Agent

A Python-based automation agent that scrapes upcoming World Cube Association (WCA) competitions in the Pacific Northwest region and sends formatted email notifications.

## 🎯 Purpose

Automatically monitor and notify about cubing competitions in:
- Washington State
- Oregon State
- British Columbia, Canada

## ✨ Features

- 🔍 Scrapes WCA competition data from official website
- 📧 Sends formatted HTML email with competition details
- 📅 Filters competitions by region and date
- 🔄 Scheduled automated runs (weekly/custom)
- ⚡ Fast and lightweight
- 🛡️ Error handling and retry logic

## 📋 Prerequisites

- Python 3.8+
- Gmail account (or other SMTP server)
- Internet connection

## 🚀 Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/wca-competition-agent.git
cd wca-competition-agent
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Configure settings:
```bash
cp config.example.json config.json
```

Edit `config.json` with your details:
```json
{
  "regions": ["Washington", "Oregon", "British Columbia"],
  "base_url": "https://www.worldcubeassociation.org/competitions",
  "email_from": "your-email@gmail.com",
  "email_to": ["recipient@example.com"],
  "smtp_server": "smtp.gmail.com",
  "smtp_port": 587,
  "smtp_password": "your-app-password",
  "date_range_months": 6
}
```

## 🔐 Gmail Setup

For Gmail users:
1. Enable 2-Factor Authentication on your Google account
2. Generate an App Password:
   - Go to Google Account Settings → Security → 2-Step Verification → App passwords
   - Select "Mail" and your device
   - Copy the 16-character password
3. Use this App Password in `config.json`

## 📖 Usage

### One-time Run
```bash
python wca_agent.py
```

### Scheduled Run (Linux/Mac - Cron)
```bash
crontab -e
```

Add this line for weekly Sunday runs at 8 PM:
```
0 20 * * 0 /usr/bin/python3 /path/to/wca_agent.py
```

### Scheduled Run (Windows - Task Scheduler)
1. Open Task Scheduler
2. Create Basic Task
3. Set trigger (e.g., Weekly, Sunday, 8:00 PM)
4. Set action: Start a program → `python.exe`
5. Add arguments: `/path/to/wca_agent.py`

## 📂 Project Structure
```
wca-competition-agent/
├── README.md
├── requirements.txt
├── config.json
├── config.example.json
├── wca_agent.py           # Main script
├── scraper.py             # Web scraping logic
├── email_sender.py        # Email formatting and sending
├── utils.py               # Helper functions
└── logs/
    └── agent.log          # Execution logs
```

## 📧 Email Output Example

**Subject**: Upcoming WCA Competitions - WA/OR/BC - February 2026

The email includes:
- Total competition count
- Competitions grouped by state/province
- Competition name, dates, location
- Registration details (fee, limit, status)
- Events offered
- Direct links to WCA competition pages

## 🛠️ Configuration Options

| Option | Description | Default |
|--------|-------------|---------|
| `regions` | Areas to search | WA, OR, BC |
| `date_range_months` | How far ahead to search | 6 months |
| `email_from` | Sender email | Required |
| `email_to` | Recipient email(s) | Required |
| `smtp_server` | Mail server | smtp.gmail.com |
| `smtp_port` | Mail server port | 587 |

## 🔧 Troubleshooting

### Email not sending
- Verify SMTP credentials
- Check App Password (not regular password)
- Ensure "Less secure app access" is enabled (if applicable)

### No competitions found
- Check if regions are spelled correctly
- Verify WCA website is accessible
- Check date range settings

### Scraping errors
- WCA website structure may have changed
- Check `logs/agent.log` for details
- Verify internet connection

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details

## ⚠️ Disclaimer

This is an unofficial tool. Always verify competition details on the official WCA website before registering.

## 🔗 Links

- [World Cube Association](https://www.worldcubeassociation.org/)
- [WCA Competitions](https://www.worldcubeassociation.org/competitions)
- [WCA Regulations](https://www.worldcubeassociation.org/regulations)

## 📝 Changelog

### v1.0.0 (2026-02-05)
- Initial release
- Support for WA, OR, BC regions
- HTML email formatting
- Scheduled automation

## 👤 Author

Your Name - Vijendhar Reddy Vontela (@vijendharv)

## 🙏 Acknowledgments

- World Cube Association for maintaining competition data
- The speedcubing community