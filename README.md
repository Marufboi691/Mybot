from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, CallbackQueryHandler, ContextTypes
from telegram.constants import ChatMemberStatus

REQUIRED_CHANNEL = "@Kakashi_vip"  # Kanal username
CHANNEL_LINK = "https://t.me/Kakashi_vip"  # Kanal link

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id
    try:
        member = await context.bot.get_chat_member(REQUIRED_CHANNEL, user_id)
        if member.status in [ChatMemberStatus.MEMBER, ChatMemberStatus.ADMINISTRATOR, ChatMemberStatus.OWNER]:
            await update.message.reply_text("Xush kelibsiz! Botdan foydalanishingiz mumkin.")
        else:
            keyboard = InlineKeyboardMarkup([
                [InlineKeyboardButton("1-Kanal", url=CHANNEL_LINK)],
                [InlineKeyboardButton("Tekshirish", callback_data="check")]
            ])
            await update.message.reply_text(
                "Uzr do'stim, siz kanallarga to'liq obuna bo'lmagansiz ✅",
                reply_markup=keyboard
            )
    except:
        keyboard = InlineKeyboardMarkup([
            [InlineKeyboardButton("1-Kanal", url=CHANNEL_LINK)],
            [InlineKeyboardButton("Tekshirish", callback_data="check")]
        ])
        await update.message.reply_text(
            "Uzr do'stim, siz kanallarga to'liq obuna bo'lmagansiz ✅",
            reply_markup=keyboard
        )

# Tekshirish tugmasi bosilganda
async def check_subscription(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    user_id = query.from_user.id
    member = await context.bot.get_chat_member(REQUIRED_CHANNEL, user_id)

    if member.status in [ChatMemberStatus.MEMBER, ChatMemberStatus.ADMINISTRATOR, ChatMemberStatus.OWNER]:
        await query.edit_message_text("Xush kelibsiz! Endi botdan foydalanishingiz mumkin.")
    else:
        await query.edit_message_text("Uzr do'stim, siz kanallarga to'liq obuna bo'lmagansiz ✅")

# Asosiy bot ishga tushirish qismi
app = ApplicationBuilder().token("7882385910:AAGVrkBtHrSWwbXPD55dt7VoL30XBvBtkUQ").build()
app.add_handler(CommandHandler("start", start))
app.add_handler(CallbackQueryHandler(check_subscription, pattern="check"))
app.run_polling()
