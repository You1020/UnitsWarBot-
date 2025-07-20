# UnitsWarBot
from telegram import Update, InlineKeyboardMarkup, InlineKeyboardButton
from telegram.ext import ApplicationBuilder, CommandHandler, CallbackQueryHandler, ContextTypes

BOT_TOKEN = "7959892639:AAFtKILLLLxfr9SxYSe_RxBaQfHPiMaWwrY"
ADMIN_ID = "@PV1U1"  # ← آیدی تلگرام ادمین رو اینجا بذار

# لیست یگان‌ها
special_units = [
    "👤🌑 شبح‌های تاریکی",
    "🛡️🚜 دژبان‌های آهنین",
    "🦅✈️ بال‌های تندر",
    "🎯🔫 چشمان عقاب"
]

free_units = [
    "💻🔓 سایبری ها",
    "🌾👥 سربازان خاک",
    "💣🐺 گرگ‌های انفجار",
    "🔥🏹 کمانداران آتش",
    "⚡️🩺 پزشکان رعد",
    "🦾🕵️‍♂️ چنگال سایه",
    "📡🔐 صدای فولاد",
    "🌪️🚁 بالگردهای طوفان",
    "🕵️‍♂️📡 سایه‌بان‌ها",
    "⚡️🏗️ معماران صاعقه",
    "🧠🎭 جادوگران ذهن"
]

# ✅ دستور /start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("🎮 به گیم جنگ یگان‌ها 🪖 خوش آمدید!")

# ✅ دستور /list
async def show_commands(update: Update, context: ContextTypes.DEFAULT_TYPE):
    text = (
        "📋 دستورات موجود:\n\n"
        "/start – شروع بازی و خوش‌آمدگویی\n"
        "/list – نمایش دستورات\n"
        "/units – انتخاب یگان و ثبت آن از طریق ادمین"
    )
    await update.message.reply_text(text)

# ✅ دستور /units
async def choose_unit(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = []

    # دکمه‌های یگان ویژه (پولی)
    for unit in special_units:
        keyboard.append([InlineKeyboardButton(f"{unit} (💵)", callback_data=unit)])

    # دکمه‌های یگان رایگان
    for unit in free_units:
        keyboard.append([InlineKeyboardButton(unit, callback_data=unit)])

    reply_markup = InlineKeyboardMarkup(keyboard)
    await update.message.reply_text("🪖 یکی از یگان‌ها را برای ثبت انتخاب کن:", reply_markup=reply_markup)

# ✅ هندلر دکمه‌ها
async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    unit = query.data

    if unit in special_units:
        message = (
            f"💎 شما یگان ویژه «{unit}» را انتخاب کردید.\n"
            f"برای خرید این یگان لطفاً به ادمین پیام دهید:\n👉 {ADMIN_ID}"
        )
    else:
        message = (
            f"✅ شما یگان «{unit}» را انتخاب کردید.\n"
            f"برای ثبت رایگان آن لطفاً به ادمین پیام دهید:\n👉 {ADMIN_ID}"
        )

    await query.edit_message_text(text=message)

# 🎯 اجرای ربات
app = ApplicationBuilder().token(BOT_TOKEN).build()

app.add_handler(CommandHandler("start", start))
app.add_handler(CommandHandler("list", show_commands))
app.add_handler(CommandHandler("units", choose_unit))
app.add_handler(CallbackQueryHandler(button_handler))

app.run_polling()
