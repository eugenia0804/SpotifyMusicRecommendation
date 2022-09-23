import pandas as pd

#Introduction of the program.
print('\nWelcome to the spotify music recommendation system!\nTell me a song which you like,\nI will find good musics you may enjoy.\n')


#Instruction for the rule of input.
print('\nPlease type in the official name with a capital letter of each word (except prepositions)\n')


#Input of the program.
SongName = input("Enter the name of the song:")
AuthorName = "[" + "'" + str(input("Enter the name of the artist:")) + "'" + "]"
NumberOfResult = int(input('Enter the number of results you want to get:'))


#Define two useful functions.
#Store data in specific column into a list.
def StoreDataToList(ColumnNumber):
    DataFile = pd.read_csv("/Users/caoyujia/Desktop/CSP Project/data.csv", usecols=[ColumnNumber], names=None) #Read local csv file using pandas and grab the data in specific column.
    DataFileList = DataFile.values.tolist()
    result = []
    for item in DataFileList:
        result.append(item[0])
    return result


#Used to search the entered song’s data.
def SearchData(Name, Author):
    DataFile = pd.read_csv("/Users/caoyujia/Desktop/CSP Project/data.csv")#Read local csv file using pandas.
    DataFileSelected = DataFile[(DataFile['name'] == Name) & (DataFile['artists'] == Author)] #Search for the row the target song’s information located.
    DataFileList = DataFileSelected.values.tolist()
    result = []
    for item in DataFileList:
        for i in range(len(item)):
            result.append(item[i])
    return result
#Call the “SearchData” function.
TargetSongInfo = SearchData(SongName, AuthorName)


#Create Lists of different characteristics of a particular song using function “StoreDataToLIst”.
ID = StoreDataToList(6)
Acousticness = StoreDataToList(0)
Danceability = StoreDataToList(2)
Duration = StoreDataToList(3)
Energy = StoreDataToList(4)
Instrumentalness = StoreDataToList(7)
Liveness = StoreDataToList(9)
Loudness = StoreDataToList(10)
Speechiness = StoreDataToList(15)
Tempo = StoreDataToList(16)
Valence = StoreDataToList(17)
Popularity = StoreDataToList(13)
Key = StoreDataToList(8)
Mode = StoreDataToList(11)
Year = StoreDataToList(18)

#Initialized a total score list.
TotalScore = []


#Calculate the similarity of each characteristic between the entered song and all other songs , then add the weighted scores to get the final score.
def SimilarityCalculator():
    for i in range(len(ID)):
        AcousticnessScore = (Acousticness[i] - TargetSongInfo[0])**2 #Calculate the variance.
        DanceabilityScore = (Danceability[i] - TargetSongInfo[2])**2
        DurationScore = (((Duration[i]/100000)-200000) - ((TargetSongInfo[3]/100000)-200000)) ** 2 #Make all values between 0-1.
        EnergyScore = (Energy[i]-TargetSongInfo[4])**2
        InstrumentalnessScore = (Instrumentalness[i] - TargetSongInfo[7])**2
        LivenessScore = (Liveness[i] - TargetSongInfo[9])**2
        LoudnessScore = (((Loudness[i]/60)+60) - ((TargetSongInfo[10]/60)+60))**2
        SpeechinessScore = (Speechiness[i] - TargetSongInfo[15])**2
        TempoScore = (((Tempo[i]/100)-50) - ((TargetSongInfo[16]/100)-50))**2
        ValenceScore = (Valence[i] - TargetSongInfo[17])**2
        PopularityScore = ((Popularity[i]/100) - (TargetSongInfo[13]/100))**2
        KeyScore = ((Key[i]/12) - (TargetSongInfo[8]/12))**2
        ModeScore = (Mode[i] - TargetSongInfo[11])/50
        YearScore = (((Year[i]/101)-1920) - ((TargetSongInfo[18]/101)-1920))**2
		#Eliminate songs with low popularity by giving them huge weight which makes it decisive.
        if Popularity[i] <= 20:
            EliminationScore = 10000000
        else:
            EliminationScore = 0
        Score = AcousticnessScore + DanceabilityScore + DurationScore/10 + EnergyScore + InstrumentalnessScore + LivenessScore + LoudnessScore + SpeechinessScore + TempoScore + ValenceScore + PopularityScore + KeyScore + ModeScore + YearScore*3 + EliminationScore
        TotalScore.append(Score)
        i += 1


#Display the result.
def Output(ResultList, Number):
    if ResultList == []:
        print('\nPlease Try Again and type in the official song name and artist name on spotify\n') #Inform the user to try again.
    else:
        print('\nWait for a minute...\n')
        SimilarityCalculator() #Call the similarity calculator.
        # Sorted the score list from smallest to greatest, using id as the key.
        ScoreDic = dict(zip(ID, TotalScore))
        ScoreDicFinal = sorted(ScoreDic.items(), key=lambda x: x[1], reverse=False)
        # Display the result.
        for i in range(1, Number + 1):
			 #Read the original file.
            Data = pd.read_csv("/Users/caoyujia/Desktop/CSP Project/data.csv")
#Find the specific song information.
            DataFile = Data[Data['id'] == ScoreDicFinal[i][0]]
            DataFileList = DataFile.values.tolist()
            for item in DataFileList:
                Name = item[12] #Get the name of the song.
                Artist = item[1] #Get the name of the artist.
			 #Display the result using type “string”.
            print('Result ' + str(i) + ' :"' + str(Name) + '" by' + str(Artist))

Output(TargetSongInfo, NumberOfResult) #Call the function “OutPut”.
