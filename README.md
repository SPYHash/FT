# FT
# Tic-Tac-Toe

import random

def drawBoard(board):
    #Diese Funktion gibt das ihr übergebene Spielbrett aus

    # "board" ist eine Loste von 10 strings für das spielbrett (Index 0 wird Ignoriert).
    print(board[7] + '|' + board[8] + '|' + board[9])
    print('-+-+-')
    print(board[4] + '|' + board[5] + '|' + board[6])
    print('-+-+-')
    print(board[1] + '|' + board[2] + '|' + board[3])

def inputPlayerLetter():
    #Lässt den spieler seinen Buchstaben wählen.
    #Gibt eine Liste mit dem Spielerbuchstaben als erstem und dem Computerbuchstaben als zweitem Element zurück.
    letter = ''
    while not (letter == 'X' or letter == 'O'):
        print('Do you want to be X or O?')
        letter = input().upper()

    # Das erste Element der Liste ist der Buchdstabe des Spielers , das zweite der Buchstabe des Computers.
    if letter == 'X':
        return ['X', 'O']
    else:
        return ['O', 'X']

def whoGoesFirst():
    #wählt zufällig aus, wer den ersten Zug hat.
    if random .randint(0, 1) == 0:
        return 'computer'
    else:
        return 'player'

def makeMove(board, letter, move):
    board[move] = letter

def isWinner(bo, le):
    #Die Funktion nimmt ein Spielbrett und den SpielerBuchstaben entgegen und gibt True zurück, wenn der Spieler gewonnen hat.
    #Wir verwenden "bo" und "le" statt "board" und "le", umd die Tipparbeit zu veringern.
    return ((bo[7] == le and bo[8] == le and bo[9] == le) or #Obere Zeile
    (bo[4] == le and bo[5] == le and [6] == le) or #Mittlere Zeile
    (bo[1] == le and bo[2] == le and [3] == le) or #Untere Zeile
    (bo[7] == le and bo[4] == le and [1] == le) or #Linke spalte
    (bo[8] == le and bo[5] == le and [2] == le) or #Mittlere Spalte
    (bo[9] == le and bo[6] == le and [3] == le) or #Rechte spalte
    (bo[7] == le and bo[5] == le and [3] == le) or #Diagonale
    (bo[9] == le and bo[5] == le and [1] == le)) #Diagonale

def getBoardCopy(board):
    #Kopiert die Spielerbrettliste und gibt die Kopie zurück.
    boardCopy = []
    for i in board:
        boardCopy.append(i)
    return boardCopy

def isSpaceFree(board, move):
    #Gibt True zurück, wenn der übergebene Zug auf dem übergebenen Spielbrett mögllich ist.
    return board[move] == ' '

def getPlayerMove(board):
    #lässt den spieler einen Zug eingeben
    move = ' '
    while move not in '1 2 3 4 5 6 7 8 9'.split() or not isSpaceFree(board, int(move)):
        print('What is your next move? (1-9)')
        move = input()
    return int(move)

def chooseRandomMoveFromList(board, movesList):
    # Gibt einen gültigen Zug von der übergebenen Liste auf dem übergebenen Spielbrett zurück.
    # Gibt None zurück, wenn es keinen gültigen Zug gibt.
    possibleMoves = []
    for i in movesList:
        if isSpaceFree(board, i):
            possibleMoves.append(i)

    if len(possibleMoves) != 0:
        return random.choice(possibleMoves)
    else:
        return None

def getComputerMove(board, computerLetter):
    #Ermittelt anhanddes Spielbretts und des Computerbuchtabens einen Zug und gibt ihn zurück
    if computerLetter == 'X':
        PlayerLetter = 'O'
    else:
        PlayerLetter = 'X'

    # Algorithmus für die Tic-Tac-Toe-Ki:
    #prüft als erstes, ob ein Sieg mit dem nächsten Zug möglich ist.
    for i in range(1, 10):
        boardCopy = getBoardCopy(board)
        if isSpaceFree(boardCopy, i):
            makeMove(boardCopy, computerLetter, i)
            if isWinner(boardCopy, computerLetter):
                return i

    # Prüft ob der Spieler mit seinem nächsten Zug gewinnen könnte, und blockiert diesen Zug
    for i in range(1, 10):
        boardCopy = getBoardCopy(board)
        if isSpaceFree(boardCopy, i):
            makeMove(boardCopy, playerLetter, i)
            if isWinner(boardCopy, playerLetter):
                return i

    # Versucht eine der Ecken zu belegen, wenn eine frei ist.
    move = chooseRandomMoveFromList(board, [1, 3, 7, 9])
    if move != None:
        return move

    # Vresucht die Mitte zu belegen, wenn sie frei ist.
    if isSpaceFree(board, 5):
        return 5

    # Macht einen Zug auf einer der Seiten.
    return chooseRandomMoveFromList(board, [2, 4, 6, 8])

def isBoardFull(board):
    #Gibt True zurück, wenn alle Spielfelder belegt sind, anderfalls Fals.
    for i in range(1, 10):
        if isSpaceFree(board, i):
            return False
    return True


print('Welcome to Tic-Tac-Toe!')

while True:
    # Setzt das Spielbrett zurück.
    theBoard = [' '] * 10
    playerLetter, computerLetter = inputPlayerLetter()
    turn = whoGoesFirst()
    print('The ' + turn + ' will go first.')
    gameIsPlaying = True

    while gameIsPlaying:
        if turn == 'player':
            #Zug des Spielers
            drawBoard(theBoard)
            move = getPlayerMove(theBoard)
            makeMove(theBoard, playerLetter, move)

            if isWinner(theBoard, playerLetter):
                drawBoard(theBoard)
                print('Hooray! You have won the game!')
                gameIsPlaying = False
            else:
                if isBoardFull(theBoard):
                    drawBoard(theBoard)
                    print('The game is a tie!')
                    break
                else:
                    turn = 'computer'

        else:
            #Zug des Computers
            move = getComputerMove(theBoard, computerLetter)
            makeMove(theBoard, computerLetter, move)

            if isWinner(theBoard, computerLetter):
                drawBoard(theBoard)
                print('The computer has beaten you! you lose.')
                gameIsPlaying = False
            else:
                if isBoardFull(theBoard):
                    drawBoard(theBoard)
                    print('The game is a tie!')
                    break
                else:
                    turn = 'player'

    print('Do you want to paly again? (yes or no)')
    if not input().lower().startwith('y'):
        break
                

                    
    
            
