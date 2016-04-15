"""
    --Monika Sawicka s1097259 bin 1f--
    -- 22.01.2016--
"""
from sys import exit as sluiten
from random import choice
from string import ascii_lowercase
from os import path
import time


def lijst_maken():
    """
    Functiebeschrijving: Deze functie leest een file en maakt er een
    woordenlijst van.

    :return: wordenlijst
    """
    l_woorden = open('woorden.lst', 'r')
    l_woordenlijst = l_woorden.read().split()
    l_woorden.close()
    return l_woordenlijst


def file_controleren(path_to_check):
    """
    Deze functie controleert of het bestand bestaat.
    :param path_to_check: bestands path en naam
    :return: tuple
    """
    if not path.isfile(path_to_check):
        return False, 'Het bestand \'{}\' bestaat niet.'.format(path_to_check)
    return True, ''


def woord_toevoegen(w_toegevoegde_woord, w_woordenlijst):
    """
    Functiebeschirjving: Deze functie controleert of de woord die gebruiker
    wilt toevoegen correct i en of het id de lijst nog niet voorkomt.

    :param w_toegevoegde_woord: een string die door gebruiker ingevoerd wordt
    :param w_woordenlijst: woordenlijst met alle woorden

    :return: updated woordenlijst(type: woordenlijst)
    :return: string met informatie voor de gebruiker
    """
    l_woord = ''
    if w_toegevoegde_woord not in w_woordenlijst:
        for letter in w_toegevoegde_woord:
            if letter.isalpha():
                l_woord += letter
        if l_woord in w_woordenlijst:
            verkeerde_woord = 'Dit woord komt al voor in de woordenlijst.'
        elif len(l_woord) < 3 or len(w_toegevoegde_woord) > 38:
            verkeerde_woord = 'Woorden mogen niet korter zijn dan 3 ' \
                              'letters en niet langer zijn dan 38 letters.'
        else:
            w_woordenlijst.append(l_woord)
            w_woordenlijst.sort(key=len, reverse=False)
            w_file = open('woorden.lst', 'w')
            w_file.write('\n'.join(w_woordenlijst))
            w_file.close()
            verkeerde_woord = 'Het woord "' + l_woord + '" is toegevoegd.'
    else:
        verkeerde_woord = 'Dit woord komt al voor in de woordenlijst.'

    return w_woordenlijst, verkeerde_woord


def letter_raden(w_gis, w_gekozen_woord):
    """
    Deze Functie kontroleert of door gebruiker ingvoerde letters in de woord
    zijn en voeg ze aan de string toe als ze klopen. In ander gevaaal wordt
    het ingevoerde letter verplaatst met een '*'.

    :param w_gis: door gebruiker ingevoerde letter
    :param w_gekozen_woord: random word gekozen uit het woordenlijst

    :return: een string met ingevoerde tekens
    :return: een boelien
    """
    geraden = True
    updated_woord = ''
    for letter in w_gekozen_woord:
        if letter not in w_gis:
            updated_woord += '*'
            geraden = False
        else:
            updated_woord += letter
    return updated_woord, geraden


def woord_raden(w_geraden_letter, w_gekozen_woord, w_beurten):
    """
    Deze Functie kontroleert of invoer van het gebruiker in het woord gekozen
    uit het lijst hetzelfde is en in het gevaal dat de woord niet klopt teelt
    het 3 fouten op.

    :param w_geraden_letter: door gebruike ingevoerde teken
    :param w_gekozen_woord: een random woord gekozen uit het woordenlijst
    :param w_beurten: aantal fouten

    :return: aantal fouten
    """
    if w_geraden_letter == w_gekozen_woord:
        return True, w_beurten
    else:
        w_beurten += 3
        if w_beurten > 9:
            w_beurten = 9
        return False, w_beurten


def beurten_tellen(b_letter, b_gekozen_woord, b_beurt, b_gebruikte_letters):
    """
    Deze Functie kontroleert of het door gebruiker ingevoerde letter in het
    random gekozen woord is, als het niet het gevaal is teelt het 1 fout op.

    :param b_letter: door het gebruiker ingevorde letter
    :param b_gekozen_woord: random woord gekozen uit het woordenlijst
    :param b_beurt: aantal fouten
    :param b_gebruikte_letters: letters die door het gebruiker al ingevoerd
    werden

    :return: aantal fouten
    """
    if b_letter in b_gebruikte_letters:
        b_beurt = b_beurt
    elif b_letter not in b_gekozen_woord:
        b_beurt += 1
    return b_beurt


def galg_maken(g_beurten):
    """
    Deze funtkie maakt een lijst met figuren die een galg gaan opbouwen.

    :param g_beurten: aantal fouten

    :return: lijst met fiuren voor het galg
    """
    g_figuuren = [
        '         ;         ;         ;         ;         ;         ',
        '         ;  |      ;  |      ;  |      ;  |      ; / \     ',
        '   ____  ;  |/     ;  |      ;  |      ;  |      ; / \     ',
        '   ____  ;  |/   | ;  |      ;  |      ;  |      ; / \     ',
        '   ____  ;  |/   | ;  |    O ;  |      ;  |      ; / \     ',
        '   ____  ;  |/   | ;  |    O ;  |    | ;  |      ; / \     ',
        '   ____  ;  |/   | ;  |   \O ;  |    | ;  |      ; / \     ',
        '   ____  ;  |/   | ;  |   \O/;  |    | ;  |      ; / \     ',
        '   ____  ;  |/   | ;  |   \O/;  |    | ;  |   /  ; / \     ',
        '   ____  ;  |/   | ;  |   \O/;  |    | ;  |   / \; / \     '
    ]
    return g_figuuren[g_beurten].split(';')


def gebruikte_letters(g_gis):
    """
    Deze Functie maakt een lijst met alle letters die gebruier al ingevoerd
    had.

    :param g_gis: door het gebruiker ingevoerde letter

    :return: lijst met gebruikte letters
    """
    letters = []
    for letter in ascii_lowercase:
        if letter in g_gis:
            letters.append(letter)
        else:
            letters.append('* ')
    return letters


def tijd_tellen():
    """
    Deze Functie slaat het begin tijd op

    :return: begin tijd
    """
    t_tijd = time.time()
    return t_tijd


def punten_tellen(p_tijd, p_gekozen_woord, p_beurten):
    """
    Deze Functie rekent punten van het gebruiker

    :param p_tijd: totaale tijd van het spel
    :param p_gekozen_woord: random woord uit het woordenlijst
    :param p_beurten: aantal fouten

    :return: aantal punten
    """
    p_punten = 10000 * (len(p_gekozen_woord) / ((p_tijd * p_beurten) + 1))
    p_punten = int(p_punten)
    return p_punten


def rankinglijst_maken():
    """
    Deze Functie leest een ranking file tot de lijst

    :return: ranking lijst
    """
    ranking_file = open('ranking.txt', 'r')
    lijst_ranking = []
    inhoud_bestand = ranking_file.read().splitlines()
    for rij in inhoud_bestand:
        lijst_ranking.append(rij.split(';'))
    ranking_file.close()
    return lijst_ranking


def ranking_vullen(r_lijst, r_naam, r_gekozen_woord, r_beurten, r_tijd,
                   r_punten):
    """
    Functiebeschrijving: Deze Functie maakt een ranking.
    :param r_lijst: scores lijst
    :param r_naam: speler naam
    :param r_gekozen_woord: random woord van woordenlijst
    :param r_beurten: aantal fouten
    :param r_tijd: tijd van de spel
    :param r_punten: gehaalde punten
    :return: geeft een tupel terug, eerste is gevoelde ranking lijst, tweede
     een string met rankig.
    """
    r_lengte = len(r_gekozen_woord)
    r_lijst.append(['', str(r_naam), str(r_lengte), str(r_beurten),
                    str(r_tijd), str(r_punten)])
    r_lijst.sort(key=lambda itemrij: int(itemrij[5]), reverse=True)
    positie = 1
    for item, value in enumerate(r_lijst):
        r_lijst[item][0] = str(positie)
        positie += 1
    r_lijst = r_lijst[0:10]

    return r_lijst, positie


def ranking_wegschrijven(r_ranking, r_kopje):
    """
    Functiebeschrijving: Deze Functie schrijf een rankinglijst tot een file.

    :param r_kopje: eerste regel uit het ranking file
    :param r_ranking: lijst met ranking

    """
    file = open('ranking.txt', 'w')

    r_schrijfbaar = []

    for item in r_ranking:
        r_schrijfbaar.append(';'.join(item))

    r_schrijfbaar.insert(0, ';'.join(r_kopje))

    file.write('\n'.join(r_schrijfbaar))
    file.close()


def letters_kontroleren(l_gekozen_woord, l_gebruikte_letters):
    """
    Functiebeschrijving: Deze Functie kontroleert of door een gebruiker
    ingevoerde letters in random woord uit de lijst woorkomen.

    :param l_gekozen_woord: random woord uit de wordenlijst
    :param l_gebruikte_letters: letters die gebruiker ingevoerd had

    :return: boelien value
    """
    for letter in l_gekozen_woord:
        if letter not in l_gebruikte_letters:
            return False
    return True


def woord_kontroleren(w_gis, w_gekozen_woord):
    """
    Functiebeschrijving:

    :param w_gis: door het gebruiker ingevoerde letters
    :param w_gekozen_woord: random woord uit het woordenlijst

    :return: boelien
    """
    return w_gis == w_gekozen_woord


def strip(s_naam):
    """
    Functiebeschrijving: Deze Functie strip het door gebruiker ingevoerde naam
    uit alle tekens die neit alfabetisch zijn.

    :param s_naam: s_naam door het gebruiker ingevoerde naam
    :return: gestripde naam (string)
    """
    s_return = ''
    for letter in s_naam:
        if letter.isalpha():
            s_return += letter
    return s_return


def ranking_layout():
    """
    Functiebeschrijving: Deze Functie opent een ranking file, schrijft het
    file tot de lijst, vervolgens converteert het lijst tot een string in
    aangegeven format.
    :return: string met ranking
    """
    r_ranking = open('ranking.txt', 'r')
    lijst_ranking = []
    inhoud_bestand = r_ranking.read().splitlines()
    for rij in inhoud_bestand:
        lijst_ranking.append(rij.split(';'))
    r_ranking.close()
    r_kopje = lijst_ranking[0]
    teller = 0
    r_ranking_string = '{:<10}{:<20}{:<10}{:<10}{:<10}{:<15}\n'.format(
            r_kopje[0], r_kopje[1], r_kopje[2], r_kopje[3], r_kopje[4],
            r_kopje[5])
    for rij in lijst_ranking:
        r_ranking_string += '{:<10}{:<20}{:<10}{:<10}{:<10}{:<15}\n'.format(
                [rij[0], teller][teller > 0], rij[1], rij[2], rij[3], rij[4],
                rij[5])
        teller += 1
    return r_ranking_string


def main():
    if not file_controleren('woorden.lst')[0]:
        print(file_controleren('woorden.lst')[1])
        exit()

    if not file_controleren('ranking.txt')[0]:
        print(file_controleren('ranking.txt')[1])
        exit()
    ranking_lijst = rankinglijst_maken()
    kopje = ranking_lijst[0]
    scores = ranking_lijst[1:]
    woorden_lijst = lijst_maken()

    speler_naam = ''

    while len(speler_naam) < 1:
        speler_naam = strip(input('Wat is je naam? '))

    opnieuw_vragen = 'doorgaan'
    while opnieuw_vragen == 'doorgaan':
        k_keuze = input('\r\nWat wil je doen?\r\n1. Een woord toevoegen'
                        '\r\n2. Het spel spelen\r\n3. De ranking bekijken'
                        '\r\n4. Stoppen\r\n\nMaak je keuze: ')
        if k_keuze.isdigit():

            if int(k_keuze) not in range(1, 5):
                print('Je hebt geen geldige keuze gemaakt.'
                      'Probeer het nog eens')
            elif int(k_keuze) == 1:

                toegevoegde_woord = input('Voer een woord in: ').lower()
                woorden_lijst, verkeerde_woord = \
                    woord_toevoegen(toegevoegde_woord, woorden_lijst)
                print(verkeerde_woord)
            elif int(k_keuze) == 2:
                tijd = tijd_tellen()
                woordenlijst = lijst_maken()
                gekozen_woord = choice(woordenlijst)
                gis = []
                beurt = 0
                geraden_letter = ''
                letters_vakje = gebruikte_letters(gis)
                updated_woord, woord_geraden = letter_raden(gis,
                                                            gekozen_woord)
                galg = galg_maken(beurt)
                print('{:>5}{:>41}\n'
                      '{:>5}{:^60}\n'
                      '{:>5}\n'
                      '{:>5}{:^60}\n'
                      '{:>5}\n'
                      '{:>5}{:^60}\n'
                      '{:>5}\n'
                      '{:^40}\n'
                      '{:^40}'.format(
                        'Galgje:', 'Gebruikte letters',
                        galg[0], ' '.join(letters_vakje[:9]),
                        galg[1],
                        galg[2], ' '.join(letters_vakje[9:18]),
                        galg[3],
                        galg[4], ' '.join(letters_vakje[18:26]),
                        galg[5],
                        'Het te raden woord:',
                        ' '.join(updated_woord)))

                while not woord_geraden and beurt < 10:
                    if beurt < 9:
                        geraden_letter = input(
                                'Type een letter of een woord: ')
                        if len(geraden_letter) > 1:
                            woord_geraden, beurt = woord_raden(geraden_letter,
                                                               gekozen_woord,
                                                               beurt)
                        else:
                            gis.append(geraden_letter)
                            updated_woord, woord_geraden = \
                                letter_raden(gis, gekozen_woord)
                            if geraden_letter not in gekozen_woord:
                                beurt += 1

                        letters_gebruikt = gebruikte_letters(gis)
                        galg = galg_maken(beurt)

                        print('{:>5}{:>41}\n'
                              '{:>5}{:^60}\n'
                              '{:>5}\n'
                              '{:>5}{:^60}\n'
                              '{:>5}\n'
                              '{:>5}{:^60}\n'
                              '{:>5}\n'
                              '{:^40}\n'
                              '{:^40}'.format(
                                'Galgje:', 'Gebruikte letters',
                                galg[0], ' '.join(letters_gebruikt[:9]),
                                galg[1],
                                galg[2], ' '.join(letters_gebruikt[9:18]),
                                galg[3],
                                galg[4], ' '.join(letters_gebruikt[18:26]),
                                galg[5],
                                'Het te raden woord:',
                                ' '.join(updated_woord)))
                    else:
                        letters_kontroleren(gekozen_woord,
                                            gis)
                        woord_geraden = woord_kontroleren(geraden_letter,
                                                          gekozen_woord)
                        break
                if woord_geraden:
                    tijd2 = tijd_tellen()
                    totaale_tijd = round(tijd2 - tijd)
                    punten = punten_tellen(totaale_tijd, gekozen_woord, beurt)
                    minuten, seconden = divmod(totaale_tijd, 60)
                    eind_tijd = "{}m{}s".format(minuten, seconden)
                    lijst_ranking, positie = ranking_vullen(scores,
                                                            speler_naam,
                                                            gekozen_woord,
                                                            beurt, eind_tijd,
                                                            punten)
                    print('Je hebt het geraden! Het word was ', gekozen_woord)
                    print('Je hebt ', punten, ' punten gescoord')
                    print('Daarmee kom je op plaats ', positie,
                          'in de ranking')
                    ranking_wegschrijven(lijst_ranking, kopje)
                    print(ranking_layout())
                else:
                    print('Helaas, je hebt het woord niet geraden. Het woord'
                          ' was ', gekozen_woord, '.')
            elif int(k_keuze) == 3:
                print(ranking_layout())
            elif int(k_keuze) == 4:
                sluiten()
                opnieuw_vragen = 'stop'
        else:
            print('Je heb verkeerde keuze gemaakt. Probeer het nog eens.')


main()
