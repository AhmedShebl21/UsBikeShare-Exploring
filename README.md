# UsBikeShare-Exploring
import time
import pandas as pd
import numpy as np

CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

def get_filters():
    """
    Asks user to specify a city, month, and day to analyze.
    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    """
    print('Hello! Let\'s explore some US bikeshare data!')
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs
    cities = ['chicago','new york city','washington']
    city = input("which city ? chicago , new york city , washington ? ").lower()
    while city not in cities :
        print('invalid name')
        city = input("which city ? chicago , new york city , washington ?").lower()
      
    
    # TO DO: get user input for month (all, january, february, ... , june)
    months = ['january', 'february', 'march', 'april', 'may', 'june','all']
    month = input (' which month ? from january to june >>').lower()
    while month not in months :
        print ('invalid input')
        month = input (' which month ? from january to june >>').lower()
        
        

    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    days = ['monday','tuesday','wednesday','thursday','friday','saturday','sunday','all']
    day = input (' which day ? enter day name >>').lower()
    while day not in days :
        print('invalid input')
        day = input (' which day ? enter day name >>').lower()
        
        
    print('-'*40)
    
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.
    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    # load data file into a dataframe
    df = pd.read_csv(CITY_DATA[city])

    # convert the Start Time column to datetime
    df['Start Time'] = pd.to_datetime(df['Start Time'])

    # extract month and day of week from Start Time to create new columns
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.day_name()


    # filter by month if applicable
    if month != 'all':
        # use the index of the months list to get the corresponding int
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1

        # filter by month to create the new dataframe
        df = df[df['month'] == month]

    # filter by day of week if applicable
    if day != 'all':
        # filter by day of week to create the new dataframe
        df = df[df['day_of_week'] == day.title()]

    return df


def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    print(' most_common_month =', df ['month'].mode()[0])

    # TO DO: display the most common day of week
    print (' most_common_day =', df['day_of_week'].mode()[0])

    # TO DO: display the most common start hour
    df['hour'] = df['Start Time'].dt.hour
    print('The most common start hour is:',df['hour'].mode()[0])

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    print('The most popular start station is:',df['Start Station'].mode()[0])

    # TO DO: display most commonly used end station
    print('The most popular end station is:',df['End Station'].mode()[0])

    # TO DO: display most frequent combination of start station and end station trip
    df ['common_trip'] = df['Start Station'] + ' >>>> ' + df['End Station']
    print('The most popular trip is: ',df['common_trip'].mode()[0])

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    print('total travel time is :',df ['Trip Duration'].sum())

    # TO DO: display mean travel time
    print('Mean for travel time is :',df ['Trip Duration'].mean())


    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    print(df['User Type'].value_counts())

    # TO DO: Display counts of gender
    if 'Gender' in df.columns:
     print(df['Gender'].value_counts())
    # TO DO: Display earliest, most recent, and most common year of birth
    if 'Gender' in df.columns :
     print ('most common year of birth is : ', df['Birth Year'].mode()[0])
     print ('most recent  year of birth is :', df['Birth Year'].max())
     print ('earliest  year of birth is :', df['Birth Year'].min())

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)
    
def display_row_data(df):
    start = 0
    request = input (' would u like to display data ?? ').lower()
    while request == 'yes' :
     print(df.iloc[start :start +5])
     start += 5 
     request = input (' would u like to display data ?? ').lower()

def main():
    while True:
        city, month, day =  get_filters()
        df = load_data(city, month, day)
        print(df.shape) 
        
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df)
        display_row_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
  	main()
