from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.label import Label
from kivy.uix.popup import Popup
from kivy.core.window import Window

# تنظیم اندازه پنجره (مثل گوشی)
Window.size = (360, 640)

class MainScreen(BoxLayout):
    def __init__(self, **kwargs):
        super(MainScreen, self).__init__(**kwargs)
        self.orientation = 'vertical'
        self.padding = 10
        self.spacing = 10
        
        # بخش عنوان و منو
        self.create_header()
        
        # بخش اصلی
        self.create_main_content()
        
        # بخش پایین
        self.create_bottom_buttons()
    
    def create_header(self):
        # Header layout
        header = BoxLayout(size_hint_y=0.1)
        
        # عنوان
        title = Label(
            text='MinFinder | والت یاب',
            color=(0, 1, 1, 1),  # آبی روشن
            font_size=20,
            bold=True
        )
        header.add_widget(title)
        
        self.add_widget(header)
    
    def create_main_content(self):
        # بخش اصلی
        main_content = BoxLayout(orientation='vertical', spacing=20, size_hint_y=0.6)
        
        # نمایش وضعیت
        status_layout = BoxLayout(size_hint_y=0.2)
        level_label = Label(text='LEVEL:-', color=(1, 1, 1, 1))
        day_label = Label(text='DAY:-', color=(1, 1, 1, 1))
        speed_label = Label(text='SPEED:-', color=(1, 1, 1, 1))
        
        status_layout.add_widget(level_label)
        status_layout.add_widget(day_label)
        status_layout.add_widget(speed_label)
        main_content.add_widget(status_layout)
        
        # فیلد کاربر
        user_label = Label(
            text='USER: --',
            color=(1, 1, 1, 1),
            size_hint_y=0.3,
            canvas_before=self.draw_user_box
        )
        main_content.add_widget(user_label)
        
        # دکمه Wallet Generator
        wallet_btn = Button(
            text='Wallet Generator',
            size_hint_y=0.3,
            background_color=(0.1, 0.1, 0.1, 1),
            color=(1, 1, 1, 1)
        )
        wallet_btn.bind(on_press=self.wallet_generator)
        main_content.add_widget(wallet_btn)
        
        self.add_widget(main_content)
    
    def draw_user_box(self, canvas):
        # رسم کادر دور فیلد کاربر
        pass
    
    def create_bottom_buttons(self):
        # بخش پایین با دکمه‌ها
        bottom = BoxLayout(orientation='vertical', spacing=20, size_hint_y=0.3)
        
        # دکمه TEST
        test_btn = Button(
            text='TEST',
            background_color=(0.1, 0.1, 0.1, 1),
            color=(0, 1, 0, 1),  # سبز
            font_size=24,
            bold=True
        )
        test_btn.bind(on_press=self.test_pressed)
        bottom.add_widget(test_btn)
        
        # دکمه LOGIN
        login_btn = Button(
            text='LOGIN',
            background_color=(0.1, 0.1, 0.1, 1),
            color=(0, 1, 1, 1),  # آبی روشن
            font_size=24,
            bold=True
        )
        login_btn.bind(on_press=self.login_pressed)
        bottom.add_widget(login_btn)
        
        self.add_widget(bottom)
    
    def test_pressed(self, instance):
        popup = Popup(
            title='Test',
            content=Label(text='آزمون انجام شد!'),
            size_hint=(0.8, 0.4)
        )
        popup.open()
    
    def login_pressed(self, instance):
        popup = Popup(
            title='Login',
            content=Label(text='ورود به حساب کاربری!'),
            size_hint=(0.8, 0.4)
        )
        popup.open()
    
    def wallet_generator(self, instance):
        popup = Popup(
            title='Wallet Generator',
            content=Label(text='ساخت کیف پول جدید!'),
            size_hint=(0.8, 0.4)
        )
        popup.open()

class WalletApp(App):
    def build(self):
        return MainScreen()

if __name__ == '__main__':
    WalletApp().run()
Commands
========

This page documents all the commands and options that can be passed to
toolchain.py.


Commands index
--------------

The commands available are the methods of the ToolchainCL class,
documented below. They may have options of their own, or you can
always pass `general arguments`_ or `distribution arguments`_ to any
command (though if irrelevant they may not have an effect).

.. autoclass:: toolchain.ToolchainCL
   :members:


General arguments
-----------------

These arguments may be passed to any command in order to modify its
behaviour, though not all commands make use of them.

``--debug``
  Print extra debug information about the build, including all compilation output.

``--sdk_dir``
  The filepath where the Android SDK is installed. This can
  alternatively be set in several other ways.

``--android_api``
  The Android API level to target; python-for-android will check if
  the platform tools for this level are installed.

``--ndk_dir``
  The filepath where the Android NDK is installed. This can
  alternatively be set in several other ways.

``--ndk_version``
  The version of the NDK installed, important because the internal
  filepaths to build tools depend on this. This can alternatively be
  set in several other ways, or if your NDK dir contains a RELEASE.TXT
  containing the version this is automatically checked so you don't
  need to manually set it.


Distribution arguments
----------------------

p4a supports several arguments used for specifying which compiled
Android distribution you want to use. You may pass any of these
arguments to any command, and if a distribution is required they will
be used to load, or compile, or download this as necessary.

None of these options are essential, and in principle you need only
supply those that you need.


``--name NAME``
  The name of the distribution. Only one distribution with a given name can be created.

``--requirements LIST,OF,REQUIREMENTS`` 
  The recipes that your
  distribution must contain, as a comma separated list. These must be
  names of recipes or the pypi names of Python modules.

``--force-build BOOL``
  Whether the distribution must be compiled from scratch.

``--arch``
  The architecture to build for. You can specify multiple architectures to build for
  at the same time. As an example ``p4a ... --arch arm64-v8a --arch armeabi-v7a ...``
  will build a distribution for both ``arm64-v8a`` and ``armeabi-v7a``.

``--bootstrap BOOTSTRAP``
  The Java bootstrap to use for your application. You mostly don't
  need to worry about this or set it manually, as an appropriate
  bootstrap will be chosen from your ``--requirements``. Current
  choices are ``sdl2`` (used with Kivy and most other apps), ``webview`` or ``qt``.


.. note:: These options are preliminary. Others will include toggles
          for allowing downloads, and setting additional directories
          from which to load user dists.
