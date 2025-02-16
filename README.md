# My_hub_bot main.py
import os
import asyncio
from aiogram import Bot, Dispatcher, types
from yt_dlp import YoutubeDL

TOKEN = os.getenv("TOKEN")  # Railway использует переменные окружения

bot = Bot(token=TOKEN)
dp = Dispatcher(bot)

# Функция для скачивания видео
async def download_video(url: str):
    ydl_opts = {
        'outtmpl': 'video.mp4',
        'format': 'best'
    }
    with YoutubeDL(ydl_opts) as ydl:
        ydl.download([url])

# Обработчик сообщений
@dp.message_handler()
async def handle_message(message: types.Message):
    url = message.text
    if "pornhub.com" in url:
        await message.reply("⏳ Скачиваю видео...")
        await download_video(url)
        await message.reply_video(video=open("video.mp4", "rb"))
        os.remove("video.mp4")
    else:
        await message.reply("⚠️ Отправьте ссылку на видео.")

# Запуск бота
async def main():
    await dp.start_polling()

if __name__ == "__main__":
    asyncio.run(main())
    
