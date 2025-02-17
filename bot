import logging
from telegram import Update, InputFile
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes

# Включение логирования
logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s', level=logging.INFO)

# Токен вашего бота
TOKEN = 'YOUR_BOT_TOKEN'
# Ваш Telegram ID (куда будут отправляться идеи)
YOUR_TELEGRAM_ID = 'YOUR_TELEGRAM_ID'

# Функция для старта
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text('Привет! Пожалуйста, отправьте свою идею или медиафайл.')

# Функция для обработки текстовых сообщений и медиафайлов
async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    if update.message.text:  # Если сообщение текстовое
        idea = update.message.text
        # Отправляем идею вам на ваш Telegram ID
        await context.bot.send_message(chat_id=YOUR_TELEGRAM_ID, text=f'Идея от {update.message.from_user.username}: {idea}')
        
        # Сохраняем идею в файл (по желанию)
        with open('ideas.txt', 'a') as f:
            f.write(idea + '\n')
        
        await update.message.reply_text('Спасибо за вашу идею!')
    
    elif update.message.photo:  # Если сообщение содержит фото
        photo_file = update.message.photo[-1].file_id  # Получаем файл фото
        await context.bot.send_photo(chat_id=YOUR_TELEGRAM_ID, photo=photo_file, caption=f'Фото от {update.message.from_user.username}')
        await update.message.reply_text('Спасибо за ваше фото!')
    
    elif update.message.video:  # Если сообщение содержит видео
        video_file = update.message.video.file_id  # Получаем файл видео
        await context.bot.send_video(chat_id=YOUR_TELEGRAM_ID, video=video_file, caption=f'Видео от {update.message.from_user.username}')
        await update.message.reply_text('Спасибо за ваше видео!')

# Основная функция
def main() -> None:
    app = ApplicationBuilder().token(TOKEN).build()

    # Обработчики команд
    app.add_handler(CommandHandler("start", start))

    # Обработчик текстовых сообщений и медиафайлов
    app.add_handler(MessageHandler(filters.TEXT | filters.PHOTO | filters.VIDEO, handle_message))

    # Запуск бота
    app.run_polling()

if name == '__main__':
    main()
