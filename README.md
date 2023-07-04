from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, dispatcher
from random import choice

BotToken = "6219173982:AAGzbhqIfaaDkfx50kRj03St1y1ECF-AH0g"
Bot = Updater(token=BotToken, use_context=True)
dispatcher = Bot.dispatcher

# Core settings
states = ['r', 'p', 's']
Shaper = {"r": "💎︁",
          "p": "📜︁",
          "s": "✀︁"}

win_rates = {}


def start(update, ctx):
    win_rates[update.effective_chat.id] = {'c_wins': 0, 'u_wins': 0}
    ctx.bot.send_message(chat_id=update.effective_chat.id, text="Добро пожаловать в RPS-игру"
                                                                "\n Вы можете начать играть, введя свой шанс на камень, ножницы, бумагу (или r, p, s)"
                                                                "\n Если вы хотите выйти, просто введите exit, если вы хотите снова играть с нуля, просто введите again.")

def inputHandler(update, ctx):
    text = core(update.effective_chat.id, update.message.text)
    ctx.bot.send_message(chat_id=update.effective_chat.id, text=text)

def core(uid, text):
    while True:
        inp = text.lower()[0]
        c_choice = choice(states)
        if inp == 'e':
            win_rates[uid] = {'c_wins': 0, 'u_wins': 0}
            return 'Thank you for playing with us!'

        elif inp == 'a':
            win_rates[uid] = {'c_wins': 0, 'u_wins': 0}
            return "Restarted!"

        elif (inp == 'r' and c_choice == 'p'
        ) or (inp == 'p' and c_choice == 's'
        ) or (inp == 's' and c_choice == 'r'):
            win_rates[uid]['c_wins'] +=1

        elif (inp == 'r' and c_choice == 's'
        ) or (inp == 'p' and c_choice == 'r'
        ) or (inp == 's' and c_choice == 'p'):
            win_rates[uid]['u_wins'] +=1

        elif inp == c_choice: pass
        
        else:
            return "Wrong input! Please try again"

        return f' User Wins: {win_rates[uid]["u_wins"]} \n Computer Wins: {win_rates[uid]["c_wins"]} \n You {Shaper[inp]}  vs {Shaper[c_choice]}  Computer'

