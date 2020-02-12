

                                    Analysis of free User Centered Android and IOS Mobile Apps


     This project is about building Android and IOS mobile apps that are free to download and install.  Since the main source of revenue
consists of in-app ads, it shows that our revenue for any app is highly dependant on the number of users who use our app.

     Whenever more users see and engage with the ads, the more income would be generated.  Therefore our goal for this porject is to
analyze data to help our developers understand what type of apps are likely to attract more users.
_____________________________________________________________________________________________________________________________

We open the two data sets the AppleStore.csv and the googleplaystore.csv

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)

or

dataset = open('AppStore.csv', 'r')
print(dataset.read())

_______________________________________________________________________________________________________________

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

or

googledataset = open('googleplaystore.csv', 'r')
print(googledataset.read())

______________________________________________________________________________________________________________

Here we are exploring the two datasets using the explore_data() function

def explore_data(dataset, start, end, rows_and_columns=False):
    dataset_slice = dataset[start:end]    
    for row in dataset_slice:
        print(row)
        print('\n') # adds a new (empty) line after each row

    if rows_and_columns:
        print('Number of rows:', len(dataset))
        print('Number of columns:', len(dataset[0]))

__________________________________________________________________________________________________________

# below we are printing the first few rows of each data set.

dataset=open('AppStore.csv', 'r')
print(dataset.readlines()[0:6])


googledataset=open('googleplaystore.csv', 'r')
print(googledataset.readlines()[0:3])

_________________________________________________________________________________________________________________
               
# The number of rows and columns of each data set is as follows and the function assumes the argument
# for the dataset parameter doesn't have a header row

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)
len(dataset)
len(dataset[0])

No. of rows:  7197
No .of column: 16

_________________________________________________________________________________________________________________

googledataset=open('googleplaystore.csv', 'r')
len(googledataset.readlines()[1:])

Number_of_columns = len(googledataset[0])
print(Number_of_columns)

No. of rows:  10841
No of column: 13
_________________________________________________________________________________________________________________

# Below we are prining the column names and trying to identify the columns that could help us with our analysis

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)

column_names = dataset[0]

print(column_names)

Analysis_Columns = print(column_names[0], ',', column_names[1], ',', column_names[4], ',', column_names[7], ',', column_names[10])


['id', 'track_name', 'size_bytes', 'currency', 'price', 'rating_count_tot', 'rating_count_ver', 'user_rating', 'user_rating_ver', 'ver', 'cont_rating', 'prime_genre', 'sup_devices.num', 'ipadSc_urls.num', 'lang.num', 'vpp_lic']

id , track_name , price , user_rating , cont_rating

_____________________________________________________________________________________________________________________

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

column_names = googledataset[0]

print(column_names)

Analysis_Columns = print(column_names[0], ',', column_names[1], ',', column_names[2], ',', column_names[4], ',', column_names[7], ',', column_names[8], ',', column_names[9])

['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']

App , Category , Rating , Size , Price , Content Rating , Genres

_____________________________________________________________________________________________________________________

Based on the discussion section, the error is at row number 10472 and index number is 2 and it is idneed incorrect as the rating should not 
exceed 5. In this case the rating is 19 so we have to delete the row.

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)
googledataset = googledataset[1:]

Wrong_data = googledataset[10472]

print(Wrong_data)

index = Wrong_data.index('19')

print('The index of 19:', index)


['Life Made WI-Fi Touchscreen Photo Frame', '1.9', '19', '3.0M', '1,000+', 'Free', '0', 'Everyone', '', '11-Feb-18', '1.0.19', '4.0 and up', '']
['App', 'Category', 'Rating', 'Reviews', 'Size', 'Installs', 'Type', 'Price', 'Content Rating', 'Genres', 'Last Updated', 'Current Ver', 'Android Ver']
The index of 19: 2


print(len(googledataset))
del googledataset[10472]


print(len(googledataset))

10840
10839

________________________________________________________________________________________________________________________

As we can see below the Google Play data set has duplicate entries which counts 1181 and names of some duplicate examples of app are alos listed
Examples of duplicate apps: ['Quick PDF Scanner + OCR FREE', 'Box', 'Google My Business', 'ZOOM Cloud Meetings',
'join.me - Simple Meetings', 'Box']

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)
googledataset = googledataset[1:]

duplicate_apps = []
unique_apps = []

for app in googledataset:
    name = app[0]
    if name in unique_apps:
        duplicate_apps.append(name)
    else:
        unique_apps.append(name)
    
print('Number of duplicate apps:', len(duplicate_apps))
print('\n')
print('Examples of duplicate apps:', duplicate_apps[:6])

Number of duplicate apps: 1181

_______________________________________________________________________________________________

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)
googledataset = googledataset[1:]

reviews_max = {}

for app in googledataset:
    name = app[0]
    n_reviews = str(app[3])
    
    if name in reviews_max and reviews_max[name] < n_reviews:
        reviews_max[name] = n_reviews
        
    elif name not in reviews_max:
        reviews_max[name] = n_reviews
        
print('Expected length:', len(googledataset) - 1181)
print('Actual length:', len(reviews_max))

Expected length: 9659
Actual length: 9659
_______________________________________________________________________________________________

Here we created a dictionary key which is unique app name with a corresponding dictionary value having the highest
number of reviews of that app.  looping through the googledataset, excluding the header row and we use 
the third endex of the each row, by changing this particular review colmn (index 3) to float data type.  After we check
if name exists in the new dictionary or not.  Finally we check if the length of the dictionary is 9659 rows after we subtract the 
duplicate entries as well as the actual length of the new dictionary we created is also 9659. Since the newly created dictionary dos 
not have any duplicates it is supposed to be the 9659 in lenght.

 

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)
googledataset = googledataset[1:]


reviews_max = {}
for app in googledataset: 
    name = app[0]
    n_reviews = float(app[3])
    
    if name in reviews_max:
        reviews_max[name] < n_reviews 
        reviews_max[name] = n_reviews
    
    elif name not in reviews_max:
        reviews_max[name] = n_reviews

print('Expected length:', len(googledataset) - 1181)
print('Actual length:', len(reviews_max))

Expected length: 9659
Actual length: 9659
_____________________________________________________________________________________________________________________________

In this case the function takes in a string and returns False if the cahracter of the string is greater than 127 
if not it returns True, which means it is a non English language.

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

def is_english(string):
    
    for character in string:
        if ord(character) > 127:
            return False
    
    return True

print(is_english('Facebook'))
print(is_english('爱奇艺PPS -《欢乐颂2》电视剧热播'))
print(is_english('Docs To Go™ Free Office Suite'))
print(is_english('Instachat 😜'))

True
False
False
False

________________________________________________________________________________________________________________________________

What we did above does not work for all the English apps as some app names use images or symbols and the like which 
falls outside of the ASCII range (more than three non-ASCII) characters. In addition to this we check if these app names are '
detected as English or non-English for each data set and explore the data sets .

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

def is_english(string):
    non_ascii = 0
    
    for character in string:
        if ord(character) > 127:
            non_ascii += 1
    
    if non_ascii > 3:
        return False
    else:
        return True

print(is_english('Docs To Go™ Free Office Suite'))
print(is_english('Instachat 😜'))
print(is_english('爱奇艺PPS -《欢乐颂2》电视剧热播'))


True
True
FalsE



opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)


googledataset_english = []
AppStore_english = []

for app in googledataset:
    name = app[0]
    if is_english(name):
        googledataset.append(app)
        
for app in AppStore:
    name = app[1]
    if is_english(name):
        AppStore_english.append(app)
        
explore_data(googledataset_english, 0, 3, True)
print('\n')
explore_data(AppStore_english, 0, 3, True)

_____________________________________________________________________________________________________________

The code below shows loop through each dataset to isolate the free apps in a separate list and checks
the length of each data set to check how many apps are remaining.


opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)

googledataset_free_apps = []
dataset_free_apps = []

for app in googledataset:
    price = app[7]
    if price == '0':
        googledataset_free_apps.append(free_app)

for app in dataset:
    price = app[4]
    if price == '0':
        dataset_free_apps.append(free_app)

print(len(googledataset_free_apps))
print(len(dataset_free_apps))

8858
4056

_________________________________________________________________________________________________________________________

The reason why we want to find an app profile that fits both the App Store and Google Play is to determine the kinds of apps
that are likely to attract more users as our revenue is highly influenced and depends on the number of people using our apps.
Based on the code above, which isolates both dataset in a sparate list we can inspect both data sets to identify the columns 
we can also generate frequency tables to find out what the most common geners in each market are.  So according to the results shown
below the App Store is dominated by apps designed for fun (game), while google play shows more balancedland scape of both practical and 
fun (game) apps.  Therefore we would use the prime_genre column for the AppStore data set and the Geners and Category columns for the 
googleplaystore data set. 

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)

googledataset_free_apps = []
dataset_free_apps = []

for app in googledataset:
    price = app[7]
    if price == '0':
        googledataset_free_apps.append(free_app)

for app in dataset:
    price = app[4]
    if price == '0':
        dataset_free_apps.append(free_app)

display_table(dataset_free_apps, -5)

Games : 55.64595660749507
Entertainment : 8.234714003944774
Photo & Video : 4.117357001972387
Social Networking : 3.5256410256410255
Education : 3.2544378698224854
Shopping : 2.983234714003945
Utilities : 2.687376725838264
Lifestyle : 2.3175542406311638
Finance : 2.0710059171597637
Sports : 1.947731755424063
Health & Fitness : 1.8737672583826428
Music : 1.6518737672583828
Book : 1.6272189349112427
Productivity : 1.5285996055226825
News : 1.4299802761341223
Travel : 1.3806706114398422
Food & Drink : 1.0601577909270217
Weather : 0.7642998027613412
Reference : 0.4930966469428008
Navigation : 0.4930966469428008
Business : 0.4930966469428008
Catalogs : 0.22189349112426035
Medical : 0.19723865877712032



display_table(dataset_free_apps, 1)


FAMILY : 17.739043824701195
GAME : 10.56772908366534
TOOLS : 7.6195219123505975
BUSINESS : 4.442231075697211
PRODUCTIVITY : 3.944223107569721
LIFESTYLE : 3.6155378486055776
SPORTS : 3.5856573705179287
COMMUNICATION : 3.5856573705179287
MEDICAL : 3.5258964143426295
FINANCE : 3.4760956175298805
HEALTH_AND_FITNESS : 3.237051792828685
PHOTOGRAPHY : 3.117529880478088
PERSONALIZATION : 3.0776892430278884
SOCIAL : 2.908366533864542
NEWS_AND_MAGAZINES : 2.7988047808764938
SHOPPING : 2.5697211155378485
TRAVEL_AND_LOCAL : 2.450199203187251
DATING : 2.2609561752988045
BOOKS_AND_REFERENCE : 2.0219123505976095
VIDEO_PLAYERS : 1.7031872509960162
EDUCATION : 1.5139442231075697
ENTERTAINMENT : 1.4641434262948207
MAPS_AND_NAVIGATION : 1.3147410358565739
FOOD_AND_DRINK : 1.245019920318725
HOUSE_AND_HOME : 0.8764940239043826
LIBRARIES_AND_DEMO : 0.8366533864541833
AUTO_AND_VEHICLES : 0.8167330677290837
WEATHER : 0.7370517928286853
EVENTS : 0.6274900398406374
ART_AND_DESIGN : 0.6175298804780877
COMICS : 0.5976095617529881
PARENTING : 0.5776892430278884
BEAUTY : 0.5278884462151394

___________________________________________________________________________________________________________________________________________________________________

Here, we creat a function named freq_table() that takes two inputs.


opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)

opened_file = open('AppStore.csv')
from csv import reader
read_file = reader(opened_file)
dataset = list(read_file)




def freq_table(dataset, index):
    table = {}
    total = 0
    
    for row in dataset:
        total += 1
        value = row[index]
        if value in table:
            table[value] += 1
        else:
            table[value] = 1
    
    table_percentages = {}
    for key in table:
        percentage = (table[key] / total) * 100
        table_percentages[key] = percentage 
    
    return table_percentages


def display_table(dataset, index):
    table = freq_table(dataset, index)
    table_display = []
    for key in table:
        key_val_as_tuple = (table[key], key)
        table_display.append(key_val_as_tuple)
        
    table_sorted = sorted(table_display, reverse = True)
    for entry in table_sorted:
        print(entry[1], ':', entry[0])
____________________________________________________________________________________________________________________________

Below is the analyzed frequency table we generated for the prime_genere column of the App Store data set as well as the frequency table 
generated for the category and geners of googleplaystore. To find out what genres are the most popular or have the most user is by calculating the 
average number of installs for each app genre.  Since the installs information is missing for the App Store data, we use the rating_count_tot app colmn but for the 
Google Play data we use the Installs column.  Inorder to get a precise data of installs, we have to remove the comas and  pluscharacters
from the the number of installs for each gener (category).  So we can conclude from the analysis that the most common genres for the App Stor dataset are 
social networking, music and navigation.  For the google play dataset the game genre seems more popular.



#googledataset_free_apps = []
#dataset_free_apps = []

# Calculation of the average number of user ratings per app genre on the App Store. 
        
genres_ios = freq_table(dataset_free_apps, -5)

for genre in genres_ios:
    total = 0
    len_genre = 0
    for app in dataset_free_apps:
        genre_app = app[-5]
        if genre_app == genre:            
            n_ratings = float(app[5])
            total += n_ratings
            len_genre += 1
    avg_n_ratings = total / len_genre
    print(genre, ':', avg_n_ratings)

    Social Networking : 53078.195804195806
Photo & Video : 27249.892215568863
Games : 18924.68896765618
Music : 56482.02985074627
Reference : 67447.9
Health & Fitness : 19952.315789473683
Weather : 47220.93548387097
Utilities : 14010.100917431193
Travel : 20216.01785714286
Shopping : 18746.677685950413
News : 15892.724137931034
Navigation : 25972.05
Lifestyle : 8978.308510638299
Entertainment : 10822.961077844311
Food & Drink : 20179.093023255813
Sports : 20128.974683544304
Book : 8498.333333333334
Finance : 13522.261904761905
Education : 6266.333333333333
Productivity : 19053.887096774193
Business : 6367.8
Catalogs : 1779.5555555555557
Medical : 459.75



for app in ios_final:
    if app[-5] == 'Reference':
        print(app[1], ':', app[5])

Bible : 985920
Dictionary.com Dictionary & Thesaurus : 200047
Dictionary.com Dictionary & Thesaurus for iPad : 54175
Google Translate : 26786
Muslim Pro: Ramadan 2017 Prayer Times, Azan, Quran : 18418
New Furniture Mods - Pocket Wiki & Game Tools for Minecraft PC Edition : 17588
Merriam-Webster Dictionary : 16849
Night Sky : 12122
City Maps for Minecraft PE - The Best Maps for Minecraft Pocket Edition (MCPE) : 8535
LUCKY BLOCK MOD ™ for Minecraft PC Edition - The Best Pocket Wiki & Mods Installer Tools : 4693
GUNS MODS for Minecraft PC Edition - Mods Tools : 1497
Guides for Pokémon GO - Pokemon GO News and Cheats : 826
WWDC : 762
Horror Maps for Minecraft PE - Download The Scariest Maps for Minecraft Pocket Edition (MCPE) Free : 718
VPN Express : 14
Real Bike Traffic Rider Virtual Reality Glasses : 8
教えて!goo : 0
彩库宝典-【官方版】 : 0
Jishokun-Japanese English Dictionary & Translator : 0
無料で音楽や写真・カメラの裏技アプリ for iPhone7 : 0
​

categories_android = freq_table(googledataset_free_apps, 1)

for category in categories_android:
    total = 0
    len_category = 0
    for app in googledataset_free_apps:
        category_app = app[1]
        if category_app == category:            
            n_installs = app[5]
            n_installs = n_installs.replace(',', '')
            n_installs = n_installs.replace('+', '')
            total += float(n_installs)
            len_category += 1
    avg_n_installs = total / len_category
   
    print(category, ':', avg_n_installs)

    ART_AND_DESIGN : 2005195.1612903227
AUTO_AND_VEHICLES : 647317.8170731707
BEAUTY : 513151.88679245283
BOOKS_AND_REFERENCE : 9465252.512315271
BUSINESS : 2245520.3811659194
COMICS : 934769.1666666666
COMMUNICATION : 90683100.55833334
DATING : 1164270.7356828193
EDUCATION : 5729276.315789473
ENTERTAINMENT : 19516734.69387755
EVENTS : 253542.22222222222
FINANCE : 2511355.6790830945
FOOD_AND_DRINK : 2190710.008
HEALTH_AND_FITNESS : 4869225.852307692
HOUSE_AND_HOME : 1917187.0568181819
LIBRARIES_AND_DEMO : 749950.119047619
LIFESTYLE : 1477863.44077135
GAME : 33048939.16116871
FAMILY : 5742274.952835485
MEDICAL : 147563.28813559323
SOCIAL : 48184458.56849315
SHOPPING : 12588522.03488372
PHOTOGRAPHY : 32218111.54952077
SPORTS : 4860918.563888889
TRAVEL_AND_LOCAL : 27921561.32520325
TOOLS : 14968685.586928105
PERSONALIZATION : 7508854.330097088
PRODUCTIVITY : 35794644.73232323
PARENTING : 542603.6206896552
WEATHER : 5747142.162162162
VIDEO_PLAYERS : 36385565.614035085
NEWS_AND_MAGAZINES : 26677267.829181496
MAPS_AND_NAVIGATION : 5486066.590909091
______________________________________________________________________________________

opened_file = open('googleplaystore.csv')
from csv import reader
read_file = reader(opened_file)
googledataset = list(read_file)


googledataset_free_apps = []
dataset_free_apps = []

for app in googledataset:
    price = app[7]
    if price == '0':
        googledataset_free_apps.append(app)

for app in dataset:
    price = app[4]
    if price == '0':
        dataset_free_apps.append(app)


for app in googledataset_free_apps:
    if app[1] == 'COMMUNICATION' and (app[5] == '1,000,000,000+'
                                      or app[5] == '500,000,000+'
                                      or app[5] == '100,000,000+'):
        print(app[0], ':', app[5])


Messenger – Text and Video Chat for Free : 1,000,000,000+
WhatsApp Messenger : 1,000,000,000+
Google Chrome: Fast & Secure : 1,000,000,000+
Messenger Lite: Free Calls & Messages : 100,000,000+
Gmail : 1,000,000,000+
Hangouts : 1,000,000,000+
Viber Messenger : 500,000,000+
Firefox Browser fast & private : 100,000,000+
Yahoo Mail – Stay Organized : 100,000,000+
imo beta free calls and text : 100,000,000+
imo free video calls and chat : 500,000,000+
Opera Mini - fast web browser : 100,000,000+
Opera Browser: Fast and Secure : 100,000,000+
Who : 100,000,000+
WeChat : 100,000,000+
UC Browser Mini -Tiny Fast Private & Secure : 100,000,000+
Android Messages : 100,000,000+
Telegram : 100,000,000+
Google Duo - High Quality Video Calls : 500,000,000+
UC Browser - Fast Download Private & Secure : 500,000,000+
WhatsApp Messenger : 1,000,000,000+
Messenger – Text and Video Chat for Free : 1,000,000,000+
imo free video calls and chat : 500,000,000+
Viber Messenger : 500,000,000+
Hangouts : 1,000,000,000+
WeChat : 100,000,000+
Skype - free IM & video calls : 1,000,000,000+
Telegram : 100,000,000+
Who : 100,000,000+
GO SMS Pro - Messenger, Free Themes, Emoji : 100,000,000+
Android Messages : 100,000,000+
LINE: Free Calls & Messages : 500,000,000+
BBM - Free Calls & Messages : 100,000,000+
KakaoTalk: Free Calls & Text : 100,000,000+
Google Chrome: Fast & Secure : 1,000,000,000+
Firefox Browser fast & private : 100,000,000+
Opera Browser: Fast and Secure : 100,000,000+
Opera Mini - fast web browser : 100,000,000+
UC Browser Mini -Tiny Fast Private & Secure : 100,000,000+
UC Browser - Fast Download Private & Secure : 500,000,000+
Viber Messenger : 500,000,000+
Truecaller: Caller ID, SMS spam blocking & Dialer : 100,000,000+
Gmail : 1,000,000,000+
Yahoo Mail – Stay Organized : 100,000,000+
Hangouts : 1,000,000,000+
imo free video calls and chat : 500,000,000+
Viber Messenger : 500,000,000+
Skype - free IM & video calls : 1,000,000,000+
WeChat : 100,000,000+
LINE: Free Calls & Messages : 500,000,000+
KakaoTalk: Free Calls & Text : 100,000,000+
WhatsApp Messenger : 1,000,000,000+
UC Browser - Fast Download Private & Secure : 500,000,000+
Google Chrome: Fast & Secure : 1,000,000,000+
Google Duo - High Quality Video Calls : 500,000,000+
Firefox Browser fast & private : 100,000,000+
Gmail : 1,000,000,000+
Messenger – Text and Video Chat for Free : 1,000,000,000+
Messenger Lite: Free Calls & Messages : 100,000,000+
LINE: Free Calls & Messages : 500,000,000+
imo beta free calls and text : 100,000,000+
Hangouts : 1,000,000,000+
imo free video calls and chat : 500,000,000+
Skype - free IM & video calls : 1,000,000,000+
Kik : 100,000,000+
KakaoTalk: Free Calls & Text : 100,000,000+
Opera Mini - fast web browser : 100,000,000+
Opera Browser: Fast and Secure : 100,000,000+
Telegram : 100,000,000+
Truecaller: Caller ID, SMS spam blocking & Dialer : 100,000,000+
UC Browser Mini -Tiny Fast Private & Secure : 100,000,000+
Viber Messenger : 500,000,000+
WeChat : 100,000,000+
Yahoo Mail – Stay Organized : 100,000,000+
BBM - Free Calls & Messages : 100,000,000+


This project analyzes data about the App Store and Google Play mobile apps with the goal of centering on an app profile that can be profitable for both markets. The result of the analysis is mostly import for our developers to have a better knowledge and understanding on what type of apps most likely attract more users on Google and App store Mobile apps.  I started by exploring through the data and deleted some wrong data which has more than one entry.  In addition duplicate entries were removed which count more than once.  Since building apps that are free to download and install are one of our main objective we isolated only the free apps for the analysis.  Using the frequency tables we analyzed the most popular genres for each market which helped us to come up with what apps are more attractive to the users as our revenue is highly influenced by the number of people using our apps.
​​
