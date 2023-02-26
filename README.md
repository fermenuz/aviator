# aviator
game aviator signals
#include <iostream>
#include <tgbot/tgbot.h>
int main() {
    // Задаем токен бота, полученный от BotFather в Telegram
    const std::string token = "6100568882:AAG6egvv5mZJ_9CNmaKRrNseY0CrXVeK-9o";

    // Создаем объект бота и передаем ему токен
    TgBot::Bot bot(token);

    // Обработчик команды /start
    bot.getEvents().onCommand("start", [&bot](TgBot::Message::Ptr message) {
        bot.getApi().sendMessage(message->chat->id, "Привет! Я твой бот.");
    });

    // Обработчик текстовых сообщений
    bot.getEvents().onAnyMessage([&bot](TgBot::Message::Ptr message) {
        bot.getApi().sendMessage(message->chat->id, "Я получил ваше сообщение.");
    });

    // Запускаем бота
    try {
        bot.getApi().deleteWebhook();
        TgBot::TgLongPoll longPoll(bot);
        while (true) {
            longPoll.start();
        }
    } catch (const std::exception& e) {
        std::cerr << e.what() << std::endl;
    }

    return 0;
}
