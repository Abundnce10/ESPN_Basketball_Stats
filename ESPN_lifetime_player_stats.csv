import requests
from bs4 import BeautifulSoup
import csv

name = 'marvin-williams'
firstYear = 2006
lastYear = 2014
outputFile = name +'_game_stats.csv'

games = [['Date', 'DayOfWeek', 'Year', 'HomeOrAway', 'Opponent', 'GameOutcome', 'PlayersTeamScore', 'OpponentTeamScore', 'MIN', 'FGM', 'FGA', '3PM', '3PA', 'FTM', 'FTA', 'REB', 'AST', 'BLK', 'STL', 'PF', 'TO', 'PTS']]

for season in range(firstYear, lastYear+1):
    year = str(season)
    url = 'http://espn.go.com/nba/player/gamelog/_/id/2797/year/%s/%s' % (year, name)
    r = requests.get(url)
    soup = BeautifulSoup(r.text)

    # rows containing game stats have a class of 'oddrow' or 'evenrow'
    rows = ['oddrow', 'evenrow']
    for row in rows:
        trs = soup.findAll('tr', {'class':row})
        for tr in trs:
            tds = tr.findAll('td')
            if tds[0].text.strip() != 'Averages' and tds[0].text.strip() != 'Totals':
                # tds[0] : DAY m/dd
                dayOfWeek = tds[0].text.strip().split(' ')[0]
                date = tds[0].text.strip().split(' ')[1] + '/' + year
                # tds[1] : home/away OPP
                loc = tds[1].findAll('li')[0].text.strip()
                if loc == '@':
                    homeAway = 'away'
                else:
                    homeAway = 'home'
                opponent = tds[1].findAll('li')[-1].text.strip()
                # tds[2] : W/L score
                # make sure game is complete
                if type(tds[2].find('span')).__name__ == 'NoneType':
                    winLoss = 'NULL'
                else:        
                    winLoss = tds[2].find('span').text.strip()
                playerTeamScore = tds[2].find('a').text.strip().split('-')[0]
                opponentTeamScore = tds[2].find('a').text.strip().split('-')[1]
                # tds[3] : MIN
                minutes = tds[3].text.strip()
                # tds[4] : FGM-FGA
                FGM = tds[4].text.strip().split('-')[0]
                FGA = tds[4].text.strip().split('-')[1]
                # tds[6] : 3PM-3PA
                ThreePM = tds[6].text.strip().split('-')[0]
                ThreePA = tds[6].text.strip().split('-')[1]
                # tds[8] : FTM-FTA
                FTM = tds[8].text.strip().split('-')[0]
                FTA = tds[8].text.strip().split('-')[1]
                # tds[10] : REB
                REB = tds[10].text.strip()
                # tds[11] : AST
                AST = tds[11].text.strip()
                # tds[12] : BLK
                BLK = tds[12].text.strip()
                # tds[13] : STL
                STL = tds[13].text.strip()
                # tds[14] : PF
                PF = tds[14].text.strip()
                # tds[15] : TO
                TO = tds[15].text.strip()
                # tds[16] : PTS
                PTS = tds[16].text.strip()
                # append game data to games list
                games.append([date, dayOfWeek, year, homeAway, opponent, winLoss, playerTeamScore, opponentTeamScore, minutes, FGM, FGA, ThreePM, ThreePA, FTM, FTA, REB, AST, BLK, STL, PF, TO, PTS])
    # log season complete
    print "Finished:", season


## Save results to file
resultFile = open(outputFile, 'w')
wr = csv.writer(resultFile, dialect='excel')
wr.writerows(games)

