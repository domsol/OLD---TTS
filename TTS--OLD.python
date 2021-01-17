#code for tts for RR by Dom solbe <3. started 19/06/2020.
#on hold 01/10/2020
from bs4 import BeautifulSoup

from gtts import gTTS
import requests
import os
import pygame
from pygame import mixer

homeMenu = False #will turn True when use wants to go to main menu. or if false will go to last menu.... need to set up global.
searchMode = False

#welcome page
   #book list 
def bookList():
    
    global homeMenu
    global searchMode
    links = []
    unsortedlinks = str
    bookChoose = ""
    counter = 1
    # used to get the book list
    
    homeMenu = False

    source = requests.get("https://www.royalroad.com/home").text # get the main menu of website and links to all books
    soup = BeautifulSoup(source,"lxml")
    for thumb in soup.find_all('div', class_='list-thumb'): #Thanks Angela 
        unsortedlinks = (thumb.contents[1]['href']) 

        if counter == 1:
            print("book listings: \n Latest updated:\n")
        elif counter == 11:
            print(" Trending Fictions:\n")
        elif counter == 18:
            print(" Best rated:\n")

        book = unsortedlinks[15:]
        print(counter, ".", book.replace("-", " "))
        links.append(unsortedlinks)
        counter += 1


    while True:    
        menu = input (" \n please input settings, book number, trending, new, updated, Weekly popular, complete, Surpise, active-popular, best-rated, search or given menu options: ")
        
        if menu.isdigit():
            menu = int(menu)

            if (menu >= 1 and menu <= 27): 
                
            
                bookChoose = links[(menu-1)]

                books(bookChoose)
                break
                #book
            
        else:
            if (menu.capitalize() == "Settings"):
                changeSetting()
                break

            if (menu.capitalize() == "Search"):
                
                menu = input("please input wanted title")
                menu = menu.replace(" ","+")
                bookChoose = ("/fictions/search?title=" + menu)
                searchMode = True
                print(searchMode)
                
                bookList2(bookChoose, menu)
                break
                #menu

            elif (menu.capitalize() == "Best-rated"): 
                bookChoose = ("/fictions/best-rated")
                bookList2(bookChoose, menu)
                break
                #menu
            
            elif (menu.capitalize() == "Surpise"): 
                bookChoose = ("/fictions/random")
                books(bookChoose)
                break
                #book

            elif (menu.capitalize() == "Trending"):
                bookChoose = ("/fictions/trending")
                bookList2(bookChoose, menu)
                break
                
                #menu 

            elif (menu.capitalize() == "New"):
                bookChoose = ("/fictions/new-releases")
                bookList2(bookChoose, menu)
                break
                #menu 

            elif (menu.capitalize() == "Updated"):
                bookChoose = ("/fictions/latest")
                bookList2(bookChoose, menu)
                break
                #menu 

            elif (menu.capitalize() == "Complete"):
                bookChoose = ("/fictions/complete")
                bookList2(bookChoose, menu)
                break
                # menu 

            elif (menu.capitalize() == "Weekly popular"):
                bookChoose = ("/fictions/weekly-popular")
                bookList2(bookChoose, menu)
                break
                #menu

            else:
                print("wrong input")

def bookList2(bookChoose, menu):
    #this is for the menu options of bookList two as they first need to be fliter to a book page.
    #should be passed to book after
    
    links = []
    menuPage = 1
    max_ = 0 # count the max amount of books they can pick
    counter = 1
    primeBookChoose = bookChoose
    oldMax_ = 0
    global searchMode
    global homeMenu
    
    while True:
        while True:
            
            source = requests.get("https://www.royalroad.com" + bookChoose).text
            soup = BeautifulSoup(source,"lxml")
            
            for title in soup.find_all('h2', class_='fiction-title'): #Thanks Angela 
                unsortedlinks = (title.contents[1]['href'])

                book = unsortedlinks[15:]
                print(str(counter), ".", (book.replace("-", " ")))
                links.append(unsortedlinks)
                counter += 1
            
            print("\n")
            
            if (oldMax_ == len(links)):
                print ("error no books found. returning to other menu:")
                if (menuPage > 1):

                    print("!retured to last page of books", "\n")
                    menuPage = menuPage - 1
                    menuPageStr = str(menuPage)
                    

                    if searchMode == True: 
                        bookChoose = ("/fictions/search?page="  + menuPageStr + "&title=" + (menu.replace(" ", "+")) )
                        
                        
                    else:
                    
                        bookChoose = (primeBookChoose + "?page=" + menuPageStr) 
                    
                    oldMax_ = oldMax_ - max_
                    continue  
                else:
                    print("!return to main menu", "\n")
                    homeMenu = True
                    
                    oldMax_ = oldMax_ - max_
                    break 
            
        
            menu2 = input("please input 'Next' to see next page or press 'Enter' to choose book: ")
            max_ = len(links)

            if (menu2.capitalize() == "Next"): 
                
                menuPage = menuPage + 1 #this is for the ?page. it add to 2 or it repeats the same page

                menuPageStr = str(menuPage)
                if searchMode == True: 
                    bookChoose = ("/fictions/search?page="  + menuPageStr + "&title=" + (menu.replace(" ", "+")) )
                    
                else:
                    bookChoose = (primeBookChoose + "?page=" + menuPageStr) 

                

                oldMax_ = max_
                continue
            else:
                break
        
        while True:
            if (homeMenu == True):
                break
            else:
                menu = input ("please input book number: ")
                    
                if (menu >= "0" and menu <= str(max_)):
                    
                    menuNumber = int(menu)
                    bookChoose = links[menuNumber - 1]
                    
                    books(bookChoose)
                    break
                else:
                    print("wrong input")
                    continue

        if (homeMenu == True):
            print ("returning home")
            break
        else:
            print("!Returned")

            #book
            
def books(bookChoose):
    """books takes the passed books from last def (def: bookList2 / bookList) then shows the frount page to user (str: source (passed str:
    bookChoose, sub: str:soup, sub: str: bookDetails). shows 'book title', (str: bookDetails) who the book is 'by' and blerb. 
    loop for user input which inclues 'read', 'chapter list', 'back' and 'home menu'. 'read' finds first chapter href code using soup before
    calling next def and passing href code. 'chapter list' shows user all known chapters found from soup then ask's for user to input number 
    using counter to number them for user input. 'back' breaks loop sending user to past menu while setting searchMode to False. 'home menu'
    brakes the loop while setting searchMode to True, looping main loop."""

    links = []
    max_ = int
    global searchMode
    global homeMenu

    source = requests.get("https://www.royalroad.com" + bookChoose).text
    soup = BeautifulSoup(source,"lxml")

    print("\n \n \n")

    bookDetails = soup.find("h1", class_="font-white").text
    print ("book title:", bookDetails)
    bookDetails = soup.find("a", class_="font-white").text
    print ("by:   ", bookDetails, "\n")

    bookDetails = soup.find("div", class_="hidden-content").text 
    print(bookDetails)
    
    while True:

        menu = input("read, chapter list, back or home menu")
        counter = 1

        if (menu.capitalize() == "Back"):
            homeMenu == False
            break

        elif (menu.capitalize() == "Home"):
            searchMode = False
            homeMenu = True
            break

        elif (menu.capitalize() == "Chapter list"):
            for pointer in soup.find_all("tr", style="cursor: pointer"):  
                
                
                unsortedlinks = pointer["data-url"]

                book = unsortedlinks[15:]
                print(counter, ".", book.replace("-", " "))
                links.append(unsortedlinks)
                counter += 1

            max_ = len(links)

            if (max_ < 0):
                print("no chapters found")
                continue

                
            while True:
                menu = input ("please input chapter menu or type 'back': \n")

                menuNumber = int(menu)
                

                if (menuNumber >= 1 and menuNumber <= (max_)):
        
        
                    ChapterLink = links[(menuNumber-1)]
                    searchMode = False
                    chapter(ChapterLink)
                    
                    break
                elif(menu.capitalize() == "Back"):
                    pass # todo
                else:
                    print ("wrong input. try again")
                    continue
            

        elif (menu.capitalize() == "Read"):
            unsortedlinks = soup.find("div", class_="col-md-4 col-lg-3 fic-buttons text-center md-text-left").a
            ChapterLink = unsortedlinks["href"]
            searchMode = False
            chapter(ChapterLink)
            #reads book

        else:
            print("wrong input, try again.")
            continue

def chapter(ChapterLink):
    """this finds all the details on the chapter page (str: ChapterLink, sub: NextChapter) to then pass to the next def (def: reader, passed: 
    list: toRead = (str: title +  str: chapter + str: text)"""
    
    title = str
    chapter = str
    text = str
    toRead = list
    nextChapter = str
    while True:

        
        source = requests.get("https://www.royalroad.com" + ChapterLink).text
        soup = BeautifulSoup(source,"lxml")

        nextChapter = soup.find("div", class_="col-xs-6 col-md-4 col-md-offset-4 col-lg-3 col-lg-offset-6").a
        ChapterLink = nextChapter["href"]

        title = soup.find("div", class_ = "col-md-5 col-lg-6 col-md-offset-1 text-center md-text-left").h2.text
        chapter = soup.find("div", class_ = "col-md-5 col-lg-6 col-md-offset-1 text-center md-text-left").h1.text

        text = soup.find('div', class_ = "chapter-inner chapter-content").text

        print("Book: " + title + ". \n")
        print(" Chapter: " + chapter + "\n")


        

        toRead = (title + "." + chapter + "." + text + ".")
        nextChapter = reader(toRead)
        if nextChapter == True:
            continue
        else:
            break

def setting():

    file = open("config.txt", "a")
    file.close()

    while  True:
        with open('config.txt', 'r') as f:
            commandLine1 = f.readline()
            commandLine2 = f.readline()
            commandLine3 = f.readline()

        if not(commandLine1 == "Loaded=True\n"):
            print("opening")
            with open('config.txt', 'w') as f:
                f.write("Loaded=True\n")
                f.write("lang=en-uk\n")
                f.write("currectVol=0.5")
                print("setting error 1")

            tts = gTTS("Loading next chapter.", lang="en")
            tts.save("next-chapter.wav")
            continue
        
        #could have made this into a while loop and checked it too list. fix later? this is all the voices for tts
        if (commandLine2 == "lang=zh-cn\n"): #(Mandarin/China) Chinese --
            pass
        elif (commandLine2 == "lang=zh-tw\n"): #(Mandarin/Taiwan)
            pass
        elif (commandLine2 == "lang=en-us\n"): #(US) english --
            pass
        elif (commandLine2 == "lang=en-ca\n"): #(Canada)
            pass
        elif (commandLine2 == "lang=en-uk\n"): #(UK)
            pass
        elif (commandLine2 == "lang=en-gb\n"): #(Uk 2)
            pass
        elif (commandLine2 == "lang=en-au\n"): #(Australia)
            pass
        elif (commandLine2 == "lang=en-gh\n"): #(Ghana)
            pass
        elif (commandLine2 == "lang=en-in\n"): #(India)
            pass
        elif (commandLine2 == "lang=en-ie\n"): #(Ireland)
            pass
        elif (commandLine2 == "lang=en-nz\n"): #(New Zealand)
            pass
        elif (commandLine2 == "lang=en-ng\n"): #(Nigeria)
            pass
        elif (commandLine2 == "lang=en-ph\n"): #(Philippines)
            pass
        elif (commandLine2 == "lang=en-za\n"): #(South Africa)
            pass
        elif (commandLine2 == "lang=en-tz\n"): #(Tanzania)
            pass
        elif (commandLine2 == "lang=fr-ca\n"): #(french/Canada) french --
            pass
        elif (commandLine2 == "lang=fr-fr\n"): #(french/french)
            pass
        elif (commandLine2 == "lang=pt-br\n"): #(Brazil) Portuguese --
            pass
        elif (commandLine2 == "lang=pt-pt\n"): #(Portugal)
            pass
        elif (commandLine2 == "lang=es-es\n"): #(Spain) Spanish --
            pass
        elif (commandLine2 == "lang=es-us\n"): #(United States)
            pass
        else:
            with open('config.txt', 'w') as f:
                f.write("Loaded=False\n")
            continue
        
        try: 
            volLevel = float(commandLine3[11:])
        
        except ValueError:
            print("setting error 2")
            with open('config.txt', 'w') as f:
                f.write("Loaded=False\n")
            continue

        if (commandLine3[:11] == "currectVol=" and (volLevel >= 0.0 or volLevel <= 1.0)):
            break
        else:
            with open('config.txt', 'w') as f:
                f.write("Loaded=False\n")
            continue

def changeSetting():
    print("change setting mode:")
    while True:

        with open('config.txt', 'r') as f:
            commandLine1 = f.readline()
            commandLine2 = f.readline()
            commandLine3 = f.readline()

        if not((commandLine1 == "Loaded=True\n")):
            print("setting error. running fix")
            setting()
            print("fix complete")
            continue

        menu = input("please select L for langues, T for total opetions or B to return home: ")
        
        if (menu.capitalize() == "L"):
            print("current langue: ", commandLine2[5:0])
            while True:

                menu = input("please input langue as 'aa-bb' or 'Mandarin/China', if you want langue option type T, or B to go back: ")

                if (menu.capitalize() == "T" or menu.capitalize() == "Langue option"):
                    print("zh-cn = (Mandarin/China),\n zh-tw = (Mandarin/Taiwan),\n en-us = (english/US),\n en-ca = (english/Canada),\n en-uk = (english/UK),\n en-gb = (english/Uk 2),\n en-au = (english/Australia),\n en-gh = (english/Ghana),\n en-in = (english/India),\n en-ie = (english/Ireland),\n en-nz = (english/New Zealand),\n en-ng = (english/Nigeria),\n en-ph = (english/Philippines),\n en-za = (english/South Africa),\n en-tz = (english/Tanzania),\n fr-ca = (french/Canada),\n fr-fr = (french/french),\n pt-br = (Portuguese/Brazil),\n pt-pt = (Portuguese/Portugal),\n es-es = (Spain),\n es-us = (United States),")
                    continue
                elif (menu.capitalize() == "B" or menu.capitalize() == "Back"):
                    print("returning to last menu \n")
                    break
                elif (menu.capitalize() == "Zh-cn" or menu.capitalize() == "Mandarin/china"): #(Mandarin/China) Chinese --
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=zh-cn\n")
                        f.write(commandLine3)

                elif (menu.capitalize() == "Zh-tw" or menu.capitalize() == "Mandarin/taiwan"): #(Mandarin/Taiwan)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=zh-tw\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-us" or menu.capitalize() == "English/us"): #(US) english --
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-us\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-ca" or menu.capitalize() == "English/canada"): #(Canada)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-ca\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-uk" or menu.capitalize() == "English/uk"): #(UK)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-uk\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-gb" or menu.capitalize() == "English/Uk 2"): #(Uk 2)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-gb\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-au" or menu.capitalize() == "English/australia"): #(Australia)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-au\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-gh" or menu.capitalize() == "English/ghana"): #(Ghana)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-gh\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-in" or menu.capitalize() == "English/india"): #(India)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-in\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-ie" or menu.capitalize() == "English/ireland"): #(Ireland)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-ie\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-nz" or menu.capitalize() == "English/new zealand"): #(New Zealand)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-nz\n")
                        f.write(commandLine3)

                elif (menu.capitalize() == "En-ng" or menu.capitalize() == "English/nigeria"): #(Nigeria)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-ng\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-ph" or menu.capitalize() == "English/philippines"): #(Philippines)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-ph\n")
                        f.write(commandLine3)
           
                elif (menu.capitalize() == "En-za" or menu.capitalize() == "English/south africa"): #(South Africa)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-za\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "En-tz" or menu.capitalize() == "English/tanzania"): #(Tanzania)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=en-tz\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "Fr-ca" or menu.capitalize() == "French/canada"): #(french/Canada) french --
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=fr-ca\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "Fr-fr" or menu.capitalize() == "French/French"): #(french/french)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=fr-fr\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "Pt-br" or menu.capitalize() == "Portuguese/brazil"): #(Brazil) Portuguese --
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=pt-br\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "Pt-pt" or menu.capitalize() == "Portuguese/portugal"): #(Portugal)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=pt-pt\n")
                        f.write(commandLine3)
                    
                elif (menu.capitalize() == "Es-es" or menu.capitalize() == "Spanish/spain"): #(Spain) Spanish --
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=es-es\n")
                        f.write(commandLine3)
            
                elif (menu.capitalize() == "Es-us" or menu.capitalize() == "Spanish/united states"): #(United States)
                    print("changing langue to: ", menu)
                    with open('config.txt', 'w') as f:
                        f.write("Loaded=True\n")
                        f.write("lang=es-us\n")
                        f.write(commandLine3)

                else:
                    print("wrong input." , menu, "is not correct.")
            
        elif (menu.capitalize() == "T"):
            print("zh-cn = (Mandarin/China),\n zh-tw = (Mandarin/Taiwan),\n en-us = (english/US),\n en-ca = (english/Canada),\n en-uk = (english/UK),\n en-gb = (english/Uk 2),\n en-au = (english/Australia),\n en-gh = (english/Ghana),\n en-in = (english/India),\n en-ie = (english/Ireland),\n en-nz = (english/New Zealand),\n en-ng = (english/Nigeria),\n en-ph = (english/Philippines),\n en-za = (english/South Africa),\n en-tz = (english/Tanzania),\n fr-ca = (french/Canada),\n fr-fr = (french/french),\n pt-br = (Portuguese/Brazil),\n pt-pt = (Portuguese/Portugal),\n es-es = (Spain),\n es-us = (United States),")
            continue

        elif (menu.capitalize() == "B"):
            print("returning home")
            break

        else:
            print("error", menu, "is not correct")
            continue

def reader(toRead):
    """This will use pyttsx3 to turn the text to speach. it read the (list: toRead. sub: items) title, chapter name, and main body of text to the user. it
    will then ask if the user wishish to end the reading (str: endReading. if so use global homeMenu). Y returning them to home screen and N starting the
    reading of next chapter by looping (def: chapter(ChapterLink))"""

    
    global homeMenu
    completeToRead = ""
    
    while True:

        with open('config.txt', 'r') as f:
            commandLine1 = f.readline()
            commandLine2 = f.readline()
            
        if not((commandLine1 == "Loaded=True\n")):
            print("setting error. running fix")
            setting()
            print("fix complete")
            continue
        else:
            break

    d = "."
    s =  [e+d for e in toRead.split(d) if e]
    for line in s:
        line = line.strip()
        
        



    for line in s:
        if s == "":
            print("ending found")
            break
        else:
            completeToRead = completeToRead + line

    print(completeToRead)
            

    tts = gTTS(completeToRead, lang=commandLine2[5:-1])
    tts.save("Story.mp3") 
            
    print("\n Finished reading chatper")
    #to add next step

    nextChapter = loadTtsPlayer()

    return nextChapter

def loadTtsPlayer():
    global homeMenu
    nextChapter = True
    toBePlayed = str

    pygame.mixer.init()

    #need to edit it to only run the one chapter then 'next chapter'
    while True:

        with open('config.txt', 'r') as f:
            commandLine1 = f.readline()
            
        if not((commandLine1 == "Loaded=True\n")):
            print("setting error. running fix")
            setting()
            print("fix complete")
            continue
        else:
            break

    with open('config.txt', 'r') as f:
        commandLine1 = f.readline()
        commandLine2 = f.readline()
        commandLine3 = f.readline()

    userLevel = commandLine3[11:]
    userLevel = float(userLevel)

    print("playing chapter")
    dir_path = os.path.dirname(os.path.realpath(__file__))
    dir_path = dir_path
    toBePlayed = ("story.mp3")

    print(toBePlayed.capitalize())
        

    
    #https://www.youtube.com/watch?v=hpmYBY28sA8&t=551s - this is the basic frame used.
        
            

    pygame.mixer.music.load(toBePlayed)
    playing = False
    paused = False
    while True:
        print("1. play/Pause 2. return to other menu 3.Mute/Unmute 4.set sound 5.next chapter 6. reset audio")
        if userLevel == 0.0:
            print("current sound muted")
        else:
            print("currect sound: ", int(userLevel * 100))

        menu = input("please pick: ")

        if menu == "1":
            if playing == False:
                pygame.mixer.music.play()
                print("!Play")
                playing = True
            elif paused == False:
                print("!Paused")
                pygame.mixer.music.pause()
                paused = True
            else:
                pygame.mixer.music.unpause()
                print("!Play")
                paused = False

        if menu == "2":
            pygame.mixer.music.pause()
            print("!Paused")
            paused = True
            while True:
                menu = input("Do you wish to return to home menu, resume or last menu? h/r/l ")
                if menu.capitalize() == "H":
                    pygame.mixer.music.unload()
                    homeMenu = True
                    nextChapter = False
                    print("returning Home")
                    return nextChapter
                    
                elif menu.capitalize() == "R":
                    print("resuming")
                    pygame.mixer.music.resume()
                    break

                elif menu.capitalize() == "L":
                    pygame.mixer.music.unload()
                    nextChapter = False
                    print("returning last menu")
                    return nextChapter

                else:
                    print("wrong input")
                    continue


        if menu == "3":
            currentVolume = float(pygame.mixer.music.get_volume())
            if (currentVolume == 0.0):
                if userLevel == 0.0:
                    pygame.mixer.music.set_volume(0.5)
                    with open('config.txt', 'w') as f:
                        f.write(commandLine1)
                        f.write(commandLine2)
                        f.write("currectVol=0.5")
                else:
                    pygame.mixer.music.set_volume(userLevel)
                    with open('config.txt', 'w') as f:
                        f.write(commandLine1)
                        f.write(commandLine2)
                        f.write("currectVol=" + userLevel)
            else:
                pygame.mixer.music.set_volume(0.0)
                with open('config.txt', 'w') as f:
                        f.write(commandLine1)
                        f.write(commandLine2)
                        f.write("currectVol=0.0")


        if menu == "4":
            while True:
                print("currect sound: ", int(userLevel * 100))
                menu = input("please input wanted amount out of 100: ")
                menu = int(menu)
                floatMenu = float
                if (menu >= 1 and menu <= 100):
                    floatMenu = menu/100
                    print("volume set to: ", menu)
                    pygame.mixer.music.set_volume(floatMenu)
                    with open('config.txt', 'w') as f:
                            f.write(commandLine1)
                            f.write(commandLine2)
                            f.write("currectVol=" + floatMenu)
                            break
                elif menu == 0:
                    print("volume muted")
                    pygame.mixer.music.set_volume(0.0)
                    with open('config.txt', 'w') as f:
                            f.write(commandLine1)
                            f.write(commandLine2)
                            f.write("currectVol=0.0")
                            break
                else:
                    print("wrong input")

        if menu == "5":
            pygame.mixer.music.unload
            nextChapter = True
            return nextChapter

        if menu == "6":
            print("!Restarting")
            pygame.mixer.music.pause()
            paused = True
            pygame.mixer.music.rewind()

def main():
    """this will start the main loop. this is just to keep user from getting out of the loop"""

    print("running")
    while True:
        setting()
        print("setting finished")
        bookList()
        print("restarted")
main()

