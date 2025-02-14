# LazyGF - For Busy Girl Bosses 💝

Are you so busy girl bossing you forgot to text your partner all day?
is your partner so neglected they are about to dump you for an ai girlfriend? 
LazyGF is for you!
LazyGF schedules an AI generated picture of you to be sent to your partner daily with a sweet greeting to let them know that you’re thinking about them!
LazyGF can generate images of you in a variety of styles and roles (as a magician! pirate! police officer!)
Customize the greeting to include callbacks to inside jokes in your relationship!

Make sure your loved ones never feel neglected again! LazyGF lets you achieve AI girlfriend performance and consistency without the effort while you’re out girl bossing. Upgrade yourself with AI today!

LazyGF is open source on GitHub today (uses Flux Lora model on Replicate, Github Actions, and Telegram API and takes about ~10 min to set up!)
💕Happy Valentine’s Day💕! (and as always happy coding!)
(individual results may vary. LazyGF is not meant to be a substitute for real human connection. Please only use photos of people you have the rights to the likeness of! 🙏)



LazyGF automatically sends daily AI-generated pictures of you (in amazing styles and scenarios!) along with sweet customized messages to your partner via Telegram. Keep the romance alive while you're out there crushing it! 

## ✨ Features

- 🎨 Generate AI photos of you in various roles (magician, pirate, police officer!)
- 💌 Schedule daily messages with personalized greetings
- 🎯 Include your special inside jokes in messages
- ⚡ Powered by Flux LoRA AI model
- 🤖 Runs automatically via GitHub Actions
- 📱 Delivers through Telegram

> ⚠️ **Disclaimer**: LazyGF is not meant to be a substitute for real human connection. Please only use photos of people whose likeness rights you own! Individual results may vary.

## Quick Setup (10 minutes)

1. Train your AI model with your photos
2. Set up Telegram credentials
3. Configure GitHub Actions
4. Watch the magic happen daily!

## Detailed Setup Instructions

### 1. Training Your Own Model

Use [Replicate's FLUX trainer](https://replicate.com/ostris/flux-dev-lora-trainer/train) to create your personalized model:

#### Preparing Training Data
1. Collect 15-20 high-quality photos of yourself in consistent lighting
2. ZIP your training images

#### Training on Replicate
1. Visit [FLUX trainer](https://replicate.com/ostris/flux-dev-lora-trainer/train)
2. Select/create destination model
3. Use these training parameters:
   ```python
   {
       "input_images": "your-photos.zip",
       "trigger_word": "MYMODEL",  # Choose unique identifier
       "steps": 1000,
       "learning_rate": 1e-4,
       "batch_size": 1,
       "resolution": 512,
       "enable_autocaption": true
   }
   ```

#### Training Tips
- Maintain consistent photo style
- Try 1000-2000 steps
- Include trigger word in prompts
- Save successful models to Hugging Face
- Cost: ~$0.001525/second on H100 GPU

### 2. Prerequisites

- Python 3.x
- Telegram account
- API credentials from https://my.telegram.org/apps
- Replicate API token from https://replicate.com/account

### 3. Local Setup

```bash
pip install telethon replicate requests
```

## 4. Generate Telegram Session

1. Create a file called `create_session.py`:
```python
from telethon import TelegramClient

api_id = 'your_api_id'    # From my.telegram.org/apps
api_hash = 'your_api_hash' # From my.telegram.org/apps

# Create and start the client
client = TelegramClient('session', api_id, api_hash)
client.start()
```

2. Run it and follow the Telegram authentication process:
```bash
python create_session.py
```

This will create a `session.session` file in your directory.

## 5. Split Session File for GitHub Actions

1. Create or use the existing `split_session.py`:
```python
import base64

CHUNK_SIZE = 48000  # GitHub has a 48KB limit per secret

def split_session_file(filename):
    with open(filename, 'rb') as f:
        data = f.read()
    
    b64_data = base64.b64encode(data).decode()
    chunks = [b64_data[i:i+CHUNK_SIZE] for i in range(0, len(b64_data), CHUNK_SIZE)]
    
    print(f"Number of chunks: {len(chunks)}")
    for i, chunk in enumerate(chunks):
        print(f"\nCHUNK_{i+1}:")
        print(chunk)

if __name__ == "__main__":
    split_session_file('session.session')
```

2. Run it to get your session chunks:
```bash
python split_session.py
```

## 6. GitHub Repository Setup

1. Create a new GitHub repository
2. Push these files to your repository:
   - `telegram-user-account-script.py`
   - `.github/workflows/daily-telegram.yml`
   - `.gitignore`
   - `split_session.py`
   - `README.md`

## 7. Set Up GitHub Actions Secrets

1. Go to your GitHub repository → Settings → Secrets and variables → Actions
2. Add these secrets:

| Secret Name | Description |
|------------|-------------|
| `API_ID` | Your Telegram API ID (number from my.telegram.org) |
| `API_HASH` | Your Telegram API hash (from my.telegram.org) |
| `REPLICATE_API_TOKEN` | Your Replicate API token |
| `TELEGRAM_SESSION_1` | First chunk from split_session.py output |
| `TELEGRAM_SESSION_2` | Second chunk from split_session.py output |

## 8. Configure Phone Numbers

In `telegram-user-account-script.py`, update these values:
```python
phone_number = '+1234567890'      # Your phone number
recipient_user_id = '+1234567890'  # Recipient's phone number
```

## 9. Test the GitHub Action

1. Go to your repository's Actions tab
2. Click on "Daily Telegram Image Sender" workflow
3. Click "Run workflow" → "Run workflow"
4. Monitor the workflow run for any errors

## Schedule Configuration

The bot runs daily at 12:00 UTC. To change this, modify in `.github/workflows/daily-telegram.yml`:
```yaml
on:
  schedule:
    - cron: '0 12 * * *'  # Modify this line
```

### Viewing Logs
1. Go to Actions tab
2. Click the failed workflow run
3. Expand the failed step
4. Check logs for specific error messages


## Project Structure
```
├── telegram-user-account-script.py  # Main script
├── split_session.py                 # Splits session for GitHub
├── .github/
│   └── workflows/
│       └── daily-telegram.yml       # GitHub Actions config
└── .gitignore                       # Prevents sensitive file commits
```

## Customization Ideas

### Role Prompts
- Magical wizard casting spells
- Space explorer on alien worlds
- Chef creating culinary masterpieces
- Superhero saving the day
- Victorian era time traveler

### Message Templates
- "Missing you while I'm crushing these deadlines! 💪💕"
- "Thinking of you between meetings! 📊😘"
- "Taking over the world but you're still on my mind! 👑💝"

## Contributing

Love LazyGF? Contributions welcome! Open an issue or PR to help make digital love notes even better! 


---
Built with 💝 for busy partners everywhere!
Check out our NSFW version of LazyGF SendNoods [here](https://github.com/ninajlu/sendnoods)

