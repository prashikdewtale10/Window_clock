# Window_clock
Clock application for simulation of android clock application made using flet framework

import datetime
import flet as ft

from flet import (
    AppBar,
    Icon,
    IconButton,
    Text,
    colors,
    icons,
    Theme,
    theme
)

LIGHT_SEED_COLOR = colors.BLUE
DARK_SEED_COLOR = colors.INDIGO

def main(page: ft.Page):
    page.title = "PyClock Web Interface"
    page.window_width = 800
    page.window_height = 650
    page.window_resizable = False
    page.theme_mode = "light"
    page.theme = theme.Theme(color_scheme_seed=LIGHT_SEED_COLOR, use_material3=True)
    page.dark_theme = theme.Theme(color_scheme_seed=DARK_SEED_COLOR, use_material3=True)

    def change_theme(e):
        page.theme_mode = "dark" if page.theme_mode == "light" else "light"
        lightMode.icon = (
            icons.WB_SUNNY_OUTLINED if page.theme_mode == "light" else icons.WB_SUNNY)
        page.update()
    lightMode = IconButton(
            icons.WB_SUNNY_OUTLINED if page.theme_mode == "light" else icons.WB_SUNNY,
            on_click=change_theme,
        )

    def show_drawer(e):
        if e.page.controls[0].controls[0].visible:
            e.page.controls[0].controls[0].visible=False
            e.page.controls[0].controls[1].visible=False
        else:
            e.page.controls[0].controls[0].visible=True
            e.page.controls[0].controls[1].visible=True
        e.page.update()

    def alarm_list(e):
        ...

    def add_alarm(e):
        def change_time(e):
            print(f"Time picker changed, value (minute) is {time_picker.value.minute}")

        def dismissed(e):
            print(f"Time picker dismissed, value is {time_picker.value}")

        print("callback handler for add alarm event...")
        time_picker = ft.TimePicker(
        confirm_text="Confirm",
        error_invalid_text="Time out of range",
        help_text="Pick your time slot",
        on_change=change_time,
        on_dismiss=dismissed,
        )
        page.overlay.append(time_picker)
        date_button = ft.ElevatedButton(
        "Pick time",
        icon=ft.icons.TIME_TO_LEAVE,
        on_click=lambda _: time_picker.pick_time(),
    )
        add_alarm_form =  ft.Column([
            date_button
        ])
        page.controls[0].controls[2].controls.clear()
        page.controls[0].controls[2].controls.append(add_alarm_form)
        page.update()

    def menu_handler(e):
        print(e.control.selected_index)


    page.appbar = ft.AppBar(
    leading=ft.IconButton(icon=icons.MENU, on_click=show_drawer),
    leading_width=40,
    # title=ft.Text(datetime.datetime.now().strftime("%d/%m/%Y, %H:%M:%S")),
    title=ft.Text("PyClock"),
    center_title=False,
    bgcolor=ft.colors.SURFACE_VARIANT,

    actions=[
        lightMode
    ],
)

    # SIDEMENU
    rail = ft.NavigationRail(
            selected_index=0,
            label_type=ft.NavigationRailLabelType.ALL,
            # extended=True,
            min_width=100,
            min_extended_width=400,
            # leading=ft.FloatingActionButton(icon=ft.icons.LIST, text="List Data"),
            group_alignment=-0.9,
            destinations=[
                ft.NavigationRailDestination(
                    icon=ft.icons.ALARM_ADD_OUTLINED, selected_icon=ft.icons.ALARM, label="Alarm",
                ),
                ft.NavigationRailDestination(
                    icon=ft.icons.NOTE_ADD_OUTLINED, selected_icon=ft.icons.NOTE_ADD, label="To-Do"
                ),
                ft.NavigationRailDestination(
                    icon=ft.icons.TIMER_OUTLINED, selected_icon=ft.icons.TIMER, label="Timer",
                ),
                ft.NavigationRailDestination(
                    icon=ft.icons.NOTES_OUTLINED, selected_icon=ft.icons.NOTES, label="Notes",
                ),
                ft.NavigationRailDestination(
                    icon=ft.icons.SETTINGS_OUTLINED,
                    selected_icon_content=ft.Icon(ft.icons.SETTINGS),
                    label_content=ft.Text("Settings"),
                ),
            ],
            on_change = menu_handler
        )
    # adding side menu to the page
    page.add(
        ft.Row(
            [
                rail,
                ft.VerticalDivider(width=1),
                # adding container in rigth of menu
                ft.Column([
                    ft.Container(
                                width=645,
                                # bgcolor="red",
                                content=ft.Column([
                                    ft.Row([
                                        ft.TextField(label="Search alarm", dense=True, height=40, width=600, ),
                                        ft.FloatingActionButton(icon=icons.ADD, height=40, width=40, on_click=add_alarm)],
                                        # alignment= ft.MainAxisAlignment.CENTER
                                        ),
                                    ft.Row([
                                        ft.Card(
                                                 content=ft.Container(
                                                    content=ft.Column([
                                                        ft.ListTile(
                                                            leading=ft.Icon(ft.icons.SCHEDULE),
                                                            title=ft.Text("Time to go for Gym ⛹🏽‍♀️⛹🏽"),
                                                            subtitle=ft.Text("06:00"),
                                                            trailing=ft.Switch(label ="", value=True)),

                                                        ft.ListTile(
                                                            leading=ft.Icon(ft.icons.SCHEDULE),
                                                            title=ft.Text("Its working time"),
                                                            subtitle=ft.Text("08:30"),
                                                            trailing=ft.Switch(label ="", value=False))
                                                        ],),
                                                    # bgcolor="green",
                                                    width=640),
                                                    )

                                        ],
                                        alignment= ft.MainAxisAlignment.CENTER,
                                        width=50)

                                ])
                            )],

                )


            ],
            expand=True,
        )
    )


ft.app(target=main)
