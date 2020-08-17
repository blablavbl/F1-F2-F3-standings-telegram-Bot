# F1-F2-F3-standings-telegram-Bot
The bot simply tells you the current standings for the appropriate category

Code is extremely bad, but I am happy with since it's my first project


from telegram.ext import Updater, CommandHandler, MessageHandler, Filters
import requests
from bs4 import BeautifulSoup
import pandas


def print_F1(update, context):
    page = requests.get("https://www.formula1.com/en/results.html/2020/drivers.html")

    soup = BeautifulSoup(page.content, "html.parser")

    classifica = soup.find(class_='resultsarchive-table')
    # print(classifica)
    # print(punti)
    nome_piloti = classifica.find_all(class_="hide-for-mobile")
    punti_piloti = classifica.find_all(class_="dark bold")
    posizione_piloti = classifica.find_all(class_="pos")
    # print(punti_piloti[])
    posizione = 1

    lista_piloti = []
    for pilota in nome_piloti:
        lista_piloti.append(pilota.get_text())

    lista_punti = []
    for punti in punti_piloti:
        lista_punti.append(punti.get_text())

    lista_classifica_generaleF1 = zip(lista_piloti, lista_punti)
    for pilota, punti in lista_classifica_generaleF1:
        update.message.reply_text(str(posizione) + " " + pilota + " " + punti)
        posizione += 1


def print_F2(update, context):
    page = requests.get("https://www.fiaformula2.com/Standings/Driver")
    soup = BeautifulSoup(page.content, "html.parser")

    classifica = soup.find(class_='table-responsive standings-table driverstandings')
    nome_piloti = classifica.find_all(class_="visible-desktop-up")
    punti_piloti = classifica.find_all(class_="total-points")
    posizione_piloti = classifica.find_all(class_="pos")
    posizione = 1


    lista_piloti = []
    for pilota in nome_piloti:
        lista_piloti.append(pilota.get_text())

    lista_punti = []
    for punti in punti_piloti:
        lista_punti.append(punti.get_text())

    lista_classifica_generaleF2 = zip(lista_piloti, lista_punti)
    for pilota, punti in lista_classifica_generaleF2:
        update.message.reply_text(str(posizione) + " " + pilota + " " + punti)
        posizione += 1


def print_F3(update, context):
    page = requests.get("https://www.fiaformula3.com/Standings/Driver")
    soup = BeautifulSoup(page.content, "html.parser")
    classifica = soup.find(class_='table-responsive standings-table driverstandings')
    nome_piloti = classifica.find_all(class_="visible-desktop-up")
    punti_piloti = classifica.find_all(class_="total-points")
    posizione_piloti = classifica.find_all(class_="pos")
    posizione = 1

    lista_piloti = []
    for pilota in nome_piloti:
        lista_piloti.append(pilota.get_text())

    lista_punti = []
    for punti in punti_piloti:
            lista_punti.append(punti.get_text())

    lista_classifica_generaleF3 = zip(lista_piloti, lista_punti)
    for pilota, punti in lista_classifica_generaleF3:
        update.message.reply_text(str(posizione) + " " + pilota + " " + punti)
        posizione += 1


TOKEN = "1043865922:AAFQi8FhRWPNY13Z4U2KktBjgx9_Xp0mfbo"


def main():
    upd = Updater(TOKEN, use_context=True)
    disp = upd.dispatcher

    disp.add_handler(CommandHandler("f1", print_F1))
    disp.add_handler(CommandHandler("f2", print_F2))
    disp.add_handler(CommandHandler("f3", print_F3))

    upd.start_polling()

    upd.idle()


if __name__ == '__main__':
    main()
