from flask import Flask
import threading
import os
import traceback

app = Flask('')
@app.route('/')
def home(): return "Bot is running"
def keep_alive(): app.run(host='0.0.0.0', port=10000)
threading.Thread(target=keep_alive, daemon=True).start()

import asyncio
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

BOT_TOKEN = os.getenv("BOT_TOKEN")
print("TOKEN CHARGE:", "OUI" if BOT_TOKEN else "NON - ERREUR ICI")

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Bot en ligne ✅")

async def main():
    try:
        print("Démarrage du bot...")
        application = ApplicationBuilder().token(BOT_TOKEN).build()
        application.add_handler(CommandHandler("start", start))
        await application.run_polling()
    except Exception as e:
        print("ERREUR CRITIQUE:", e)
        traceback.print_exc()

if __name__ == "__main__":
    asyncio.run(main())
