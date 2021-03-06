﻿# Dynamicly Radio Buttons

To generate dynamicly radio buttons, you can use **SelectionView**. 
SelectionView has 3 selection type. They're Button (*as default*), RadioButton, CheckBox.
If you set **SelectionType** to **RadioButton** and Bind ItemsSource, you've created Radio Buttons dynamicly. 

Let's start. 
<hr />
*In that sample Basic MVVM logic will be used.*

<br/>
<br/>
That's our model:


```csharp
    public class SampleClass
    {
        public int Id { get; set; }
        public string Name { get; set; }
        //Override string and return what you want to be displayed
        public override string ToString() => Name;
    }
```


...And this is ViewModel. Just Create a list of your object. And override **ToString** inside your class, to display what you want.
Declare one more property for SelectedItem. 

```csharp
      public class MainViewModel : INotifyPropertyChanged
    {
        public MainViewModel()
        {
            MyList = new ObservableCollection<SampleClass>();
            FillData();
        }
        public IList<SampleClass> MyList { get; set; }
        private SampleClass _selectedItem;
        public SampleClass SelectedItem
        {
            get => _selectedItem;
            set { _selectedItem = value; OnPropertyChanged(); }
        }

        void FillData()
        {
            for (int i = 0; i < 6; i++)
            {
                MyList.Add(new SampleClass { Id = i, Name = "Option " + i });
            }
        }

        #region INotifyPropertyChanged Implementation
        public event PropertyChangedEventHandler PropertyChanged;
        public void OnPropertyChanged([CallerMemberName]string propName = "") => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propName)); 
        #endregion
    }
```

And there's nothing for XAML. It's one line configuration is enough.
Just set SelectionType and ColumnNumber(*default is 2*), and bind ItemsSource and SelectedItem, It'll generate radiobuttons at runtime.

```xml
  <?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:Sample.InputKit"
             xmlns:input="clr-namespace:Plugin.InputKit.Shared.Controls;assembly=Plugin.InputKit"
             x:Class="Sample.InputKit.MainPage">

    <ContentPage.BindingContext>
        <local:MainViewModel/>
    </ContentPage.BindingContext>
    <ScrollView>
        <StackLayout Padding="20">

            <input:SelectionView ColumnNumber="1" SelectionType="RadioButton" ItemsSource="{Binding MyList}" SelectedItem="{Binding SelectedItem}"  />            
            
        </StackLayout>
    </ScrollView>

</ContentPage>

```
You'll get this view:

<img src="https://scontent.xx.fbcdn.net/v/t1.15752-0/p280x280/37307333_2014282508582852_3918354395636432896_n.png?_nc_cat=0&_nc_ad=z-m&_nc_cid=0&oh=b2bfd0127eaa10595f4c802af5f26298&oe=5BEC2CCE" height="480"/>


And you can see it'll send SelectedItem to your ViewModel:


<img src="https://scontent-frx5-1.xx.fbcdn.net/v/t1.15752-9/37244622_2014279031916533_1529977327767781376_n.png?_nc_cat=0&oh=a7a97bbd11d14c1a50552d4973a58f4d&oe=5BE3888E"/>

<hr />
Best Regards...
<hr />