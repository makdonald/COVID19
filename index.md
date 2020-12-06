## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/makdonald/COVID19/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
# create a list of all countries (locations)

def dataframe_intervals(interval_input = 20_000):

    all_locations = covid.location.unique()

    interval_series = []
    days_number_concat = []

    for loc in all_locations:

        location = covid[covid['location'] == loc]
        location = location.fillna(0)

        total_cases = location['total_cases']
        date = location['date']

        def intervals (interval):
            '''Function returns list of intervals'''
            intervals = ([x for x in range(interval, int(max(total_cases)),interval)])
            intervals.insert(0,1)
            return intervals

        days_number = [None] * len(total_cases)

        for cases_value in total_cases:
            interval_list = intervals(interval_input)

            if len(interval_list) == 1:
                if cases_value == 0:
                    interval_series.append(0)
                    #break
                else:
                    interval_series.append(1)
                    #break

            if len(interval_list) > 1:
            #else:
                for index in range(0,len(interval_list)-1):
                    if cases_value == 0:
                        interval_series.append(0)
                        break
                    elif cases_value in pd.Interval(left=interval_list[index], 
                                                    right=interval_list[index+1], 
                                                    closed='left'):
                        interval_series.append(interval_list[index])
                        break
                    # values which are higher then limit of last section
                    elif cases_value > interval_list[-1]:
                        interval_series.append(interval_list[-1])
                        break
        import numpy as np

        # Create numpy array with interval series and frequencies how many times are in data frame for selected country
        freq = np.unique(interval_series, return_counts=True)

        # add number of days to None value list in correct index position
        sum_days = 0
        for days in freq[1]:
            sum_days+=days
            try:
                days_number[sum_days]=days
            except IndexError:
                break
        days_number_concat = np.concatenate((days_number_concat,days_number))

   # add last value to the list if length of two lists is different
    if (len(interval_series)-len(days_number_concat)!=0):
            interval_series.append(1)
    else:
        pass

    # Add two new columns to data frame
    covid['interval_series'] = interval_series
    covid['days_number'] = days_number_concat
    return covid
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/makdonald/COVID19/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
