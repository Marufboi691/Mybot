# Mybot
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, CallbackQueryHandler, ContextTypes
from telegram.constants import ChatMemberStatus

# === Tahrirlash ===
BOT_TOKEN = "TOKENINGNI_BU_YERGA_QOY"
REQUIRED_CHANNEL = "@KANAL_USERNAME"  # Masalan: @Kakashi_vip
CHANNEL_LINK = "https://t.me/Kakashi_vip"  # Kanal linki

# /start komandasi
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    try:
        member = await context.bot.get_chat_member(REQUIRED_CHANNEL, user_id)
        if member.status in [ChatMemberStatus.MEMBER, ChatMemberStatus.ADMINISTRATOR, ChatMemberStatus.OWNER]:
            await update.message.reply_text("Xush kelibsiz! Botdan foydalanishingiz mumkin.")
        else:
            await send_subscription_prompt(update)
    except:
        await send_subscription_prompt(update)

# Tekshirish tugmasi bosilganda
async def check_subscription(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    user_id = query.from_user.id
    member = await context.bot.get_chat_member(REQUIRED_CHANNEL, user_id)

    if member.status in [ChatMemberStatus.MEMBER, ChatMemberStatus.ADMINISTRATOR, ChatMemberStatus.OWNER]:
        await query.edit_message_text("Xush kelibsiz! Endi botdan foydalanishingiz mumkin.")
    else:
        await query.edit_message_text("Hali ham obuna bo‘lmagansiz. Iltimos, obuna bo‘ling.")

# Obuna prompti
async def send_subscription_prompt(update: Update):
    keyboard = InlineKeyboardMarkup([
        [InlineKeyboardButton("1-Kanal", url=CHANNEL_LINK)],
        [InlineKeyboardButton("Tekshirish", callback_data="check")]
    ])
    await update.message.reply_text(
        "Botdan foydalanish uchun quyidagi kanalga obuna bo‘ling:",
        reply_markup=keyboard
    )

# Botni ishga tushirish
app = ApplicationBuilder().token()7882385910:AAGVrkBtHrSWwbXPD55dt7VoL30XBvBtkUQ.build()
app.add_handler(CommandHandler("start", start))
app.add_handler(CallbackQueryHandler(check_subscription, pattern="check"))
app.run_polling()
