# cubby
My repository of all documentation related with my further education
def model_regression(country,days_number=100):
    '''
    Function returns linear and polynomial regression for selected country
    and prediction of number of total cases number of days future
    '''
    # country name saved separatly
    country_name = country
    
    # Create dataframe with dataset of Ireland
    country = df[df['location'] == country]

    # Create series of date and convert the dates to 0-based number of days from 31st December 2019
    country_date = country['date']
    country_days = [x for x in range(0,len(country_date))]

    # Repleace all nan by 0.00
    country_total_cases = country['total_cases_per_million'].fillna(0)
    
    ###################################################################
    #############         LINEAR REGRESSION       #####################
    ###################################################################

    %matplotlib inline

    from IPython.display import set_matplotlib_formats
    set_matplotlib_formats('svg')
    from scipy import stats

    slope, intercept, r, p, se = stats.linregress(country_days, country_total_cases)

    def f(x):
        return slope * x + intercept

    linear_reg = list(map(f, country_days))

    import numpy as np
    # formatting plot

    plt.style.use('ggplot')
    plt.figure(figsize=(10,6),facecolor='grey')
    plt.title('Polynomial regression for mouse weight/length')
    plt.xlabel('Days')
    plt.xticks(ticks=np.arange(0,len(country_days),step=30),rotation=45)
    plt.ylabel('Total Cases per million')
    plt.title(f'Linear Regression for total cases per million/days in {country_name}')
    plt.scatter(country_date,country_total_cases, color='b', label='Total Cases per million')
    plt.plot(country_days,linear_reg, color='red', label='Regression Line')
    plt.legend()
    plt.show()
    
    # Show the r-squared value
    r_sq = r**2
    
    ###################################################################
    #############     POLYNOMIAL REGRESSION       #####################
    ###################################################################
    
    # Do a polynomial regression analysis and plot the polynomial line on the same plot

    # Create a function which calculate y values
    mymodel = np.poly1d(np.polyfit(country_days,country_total_cases,3))

    # Create 100 x values in a range of weigt values
    x_values = np.linspace(min(country_days),max(country_days),100)

    # Calculate y values
    y_values = mymodel(x_values)
    y_values

    # formatting plot
    plt.style.use('ggplot')
    plt.figure(figsize=(10,6), facecolor='grey')
    plt.title(f'Polynomial regression for total cases per million/number of days in {country_name}')
    plt.xlabel('Days')
    plt.ylabel('Total Cases per million')
    plt.scatter(country_days,country_total_cases,color='b', label='Total Cases per million')
    plt.plot(x_values,y_values, label='Regression Line')
    plt.legend()
    
    # Show the polynomial r-squared value
    from sklearn.metrics import r2_score

    r_sq_p = r2_score(Ireland_total_cases,mymodel(Ireland_days))
      
    ###################################################################
    #############             PREDICTIONS         #####################
    ###################################################################
    
    # Prediction of total cases from today
    import datetime
    today = datetime.datetime.today()
    today_date = datetime.date.today()
    first_day = country_date.iloc[0]
    first_day = datetime.datetime.strptime(first_day, '%Y-%m-%d')
    delta = (today - first_day).days
    
    print(f'COUNTRY NAME: {country_name}')
    print('\n**********Linear Regression**********\n')
    print(f'Predicted number of total cases for {days_number} days from today ({today_date}) is {int(f(days_number+delta)):,}')
    print(f'R-squared value is equal to {r_sq:.2}')
    print('\n**********Polynomial Regression:**********\n')
    print(f'Predicted number of total cases for {days_number} days from today ({today_date}) is {int(mymodel(days_number+delta)):,}')
    print(f'R-Suared score is: {r_sq_p:.2}\n')
 
# calling function model_regression which return situation in Ireland in 150 days
model_regression('Ireland',150)
