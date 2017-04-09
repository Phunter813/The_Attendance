# The_Attendance

+ Get your Updated Attendance as well as its stats with one click.
+ Time saving as it automatically does the stats for you.
+ Also gives warnings and further information related
+ "Python 3 should be installed in you machine"
+ Then install Chromedriver [Here_is_the_link](https://sites.google.com/a/chromium.org/chromedriver/downloads) click on the latest release and download according to your machine specs
+ Copy the downloaded file to the folder you have the Python file, run the chromedriver set up the Environmental variable for it

```Python 3
from selenium import webdriver

user = input('put you username: ')
passw = input('put you password: ')

driver = webdriver.Chrome()   # change to .Firefox() if you have Mozilla
driver.get("http://erp.ncuindia.edu/Welcome_iie.aspx/Welcome_iie.aspx")
username = driver.find_element_by_name('tbUserName')
username.send_keys(user)                                         # you can put your ID directy here and in the passw section
password = driver.find_element_by_name('tbPassword')             # and remove the first two lines of input
password.send_keys(passw)       
driver.find_element_by_name('btnLogIn').click()

strings = driver.find_element_by_id('lblCourse').text
name = []
discription = strings.split()
for I in discription:
    if ':' in I:
        break
    if I.isalpha():
        name.append(I)

full_name = ' '.join(name)
print('Welcome {} \n'.format(full_name))
driver.find_element_by_id(id_='aAttandance').click()
data = driver.find_element_by_css_selector(
    'table[class="table100 table f11 noHover marB10 borTop2pxSolid altBgTrLast altBgTdLast"]')
main_contents = data.find_element_by_xpath(
    xpath='//*[@id="aspnetForm"]/div[3]/div/div/div[2]/div/div/section/div/div[2]/table/tbody')




new = main_contents.text.split('\n')

for I in new:
    I = I.split()
    Z = I[0]

sub_names = []
names = []
number_of_subjects = int(Z)
for L in range(0, number_of_subjects):
    new[L] = new[L].split()
    new[L][1] = new[L][1].split('-')
    name = new[L][1]
    del (name[0])
    new[L][1] = ''.join(name)
    sub_names.clear()
    for G in new[L]:
        G = G.replace('-', '')
        if G.isalpha():
            sub_names.append(G)
    names.append(' '.join(sub_names))

for I in range(0, number_of_subjects):
    new[I].reverse()

list_warnings = []
def minimum_attendance(data):
    for L in range(0, number_of_subjects):
        percentage_diff = 70.00 - float(int(data[L][4]) / int(data[L][5]) * 100)
        percentage_diff = float('{:.2f}'.format(percentage_diff))

        if percentage_diff > 0:
            print('____________________________________________________________________\n')
            print('Your attendance is getting short in {} by {}%'.format(names[L], percentage_diff))
            classes_needed = round(
                float('{:.2f}'.format(float(((int(data[L][5]) * 70) - (int(data[L][4]) * 100)) / 30))))

            class_Approx = (int(data[L][4]) + classes_needed) / (int(data[L][5]) + classes_needed) * 100
            if class_Approx >= 70:
                print('You need to Attend minimum {} more lectures in {} to get 70 % attendance'.format(classes_needed,
                                                                                                        names[L]))
            else:
                classes_needed = classes_needed + 1
                print('You need to attend minimum {} more lectures in {} to get 70 % attendance'.format(classes_needed,
                                                                                                        names[L]))
            print('____________________________________________________________________\n')

    decesion = input('Do you Wanna know how many classes you can leave in each subject? (Y/N) ')
    print('____________________________________________________________________\n')
    if decesion == 'Y' or decesion == 'y':
        for I in range(0, number_of_subjects):
            bunk = round(float(((int(data[I][4]) * 100) / 70) - int(data[I][5])))
            attendance = (int(data[I][4]) / (int(data[I][5]) + bunk)) * 100
            if bunk >= 1:
                if attendance >= 70:
                    print('You can leave {} lectures in {} and your attendance will be {:.2f}'.format(bunk, names[I],
                                                                                                  attendance))
                    print('____________________________________________________________________\n')
                else:
                    bunk = bunk - 1
                    if bunk > 0:
                        new_attendance = (int(data[I][4]) / (int(data[I][5]) + bunk)) * 100
                        print(
                            'You can leave {} lectures in {} and you attendance will be {:.2f}'.format(bunk, names[I],
                                                                                                   new_attendance))
                        print('____________________________________________________________________\n')

                    elif bunk == 0:
                        warning = {names[I]: attendance}
                        list_warnings.append(warning)


        dead_atten = input('Do you want to see further Attendance warnings (Y/N)')
        if dead_atten == 'Y':
            for I in list_warnings:
                for J in I:
                    print('you attendance will be {:.2f} if you leave a lecture in {}'.format(I[J], J))


minimum_attendance(new)
```
And as always we need to run this file as a batch command for that 

```Batch
@echo off
mode 1000
color A
cd/
name_of_Drive:
cd PATH
python ./selenium_click_attendance.py
pause
```
+ `name_of_drive` will be the drive in which your Python projects are being saved e.g. C:
+ To get the `PATH` right click on the python file and get the Location it would look like C:/somefolder/folder/.. 

