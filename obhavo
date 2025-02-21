import logging
import requests
from aiogram import Bot, Dispatcher, types
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton
from aiogram.utils import executor

# API kalitlari
TELEGRAM_API_TOKEN = "5001639696:AAGaGaGJHlVwYE2H4gjluH-Iv1QbyfvU8n8"
WEATHER_API_KEY = "96c924bd15608077c224dfe5fc2cbb67"

# Bot va Dispatcher yaratish
bot = Bot(token=TELEGRAM_API_TOKEN)
dp = Dispatcher(bot)

# Logging sozlamalari
logging.basicConfig(level=logging.INFO)

# Ob-havo ma’lumotlarini olish funksiyasi
def get_weather(city):
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={WEATHER_API_KEY}&units=metric&lang=uz"
    response = requests.get(url)
    data = response.json()

    if data.get("cod") != 200:
        return "❌ Shahar topilmadi! Iltimos, qayta urinib ko‘ring."

    temp = data["main"]["temp"]
    feels_like = data["main"]["feels_like"]
    humidity = data["main"]["humidity"]
    wind_speed = data["wind"]["speed"]
    description = data["weather"][0]["description"].capitalize()

    return (f"🌍 *Shahar:* {city}\n"
            f"🌡 *Harorat:* {temp}°C\n"
            f"🤔 *His qilinadi:* {feels_like}°C\n"
            f"💧 *Namlik:* {humidity}%\n"
            f"🌬 *Shamol tezligi:* {wind_speed} m/s\n"
            f"⛅ *Ob-havo:* {description}")

# Inline tugmalar (shaharlar)
def city_keyboard():
    keyboard = InlineKeyboardMarkup(row_width=2)
    cities = ["Toshkent", "Samarqand", "Buxoro", "Farg‘ona", "Namangan", "Navoiy", "Andijon", "Xiva"]

    buttons = [InlineKeyboardButton(city, callback_data=city) for city in cities]
    keyboard.add(*buttons)

    return keyboard

# /start komandasi
@dp.message_handler(commands=['start'])
async def start_command(message: types.Message):
    await message.reply("🌤 O‘zbekiston shaharlarining ob-havo ma’lumotlarini bilish uchun shaharni tanlang:", 
                        reply_markup=city_keyboard())

# Inline tugmalar bosilganda ishlov berish
@dp.callback_query_handler(lambda call: True)
async def callback_weather(call: types.CallbackQuery):
    city = call.data
    weather_info = get_weather(city)
    await call.message.answer(weather_info, parse_mode="Markdown")

# Botni ishga tushirish
if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
