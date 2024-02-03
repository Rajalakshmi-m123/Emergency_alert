import pywhatkit
import winsdk.windows.devices.geolocation as wdg
import asyncio
import tkinter as tk

async def getCoords():
    locator = wdg.Geolocator()
    pos = await locator.get_geoposition_async()
    return [pos.coordinate.latitude, pos.coordinate.longitude]

def getLoc():
    return asyncio.run(getCoords())

def generate_google_maps_link(latitude, longitude):
    return f"https://www.google.com/maps/place/{latitude},{longitude}"

def show_location():
    try:
        latitude, longitude = getLoc()
        google_maps_link = generate_google_maps_link(latitude, longitude)
        lat_label.config(text=f"Latitude: {latitude}")
        lng_label.config(text=f"Longitude: {longitude}")
        link_label.config(text=f"Google Maps Link: {google_maps_link}")
        status_label.config(text="")
    except Exception as e:
        status_label.config(text=f"Error: {e}")

def send_emergency():
    try:
        recipient_number = "+916238876300"
        latitude, longitude = getLoc()
        google_maps_link = generate_google_maps_link(latitude, longitude)
        emergency_message = f"Emergency! Help\nLocation: {google_maps_link}"
        pywhatkit.sendwhatmsg_instantly(recipient_number, emergency_message)
        status_label.config(text="Emergency message sent!")
    except Exception as e:
        status_label.config(text=f"Error: {e}")

window = tk.Tk()
window.title("My Location")
window.configure(bg="#F0F8FF")

lat_label = tk.Label(window, text="Latitude: ", font=("Arial", 16, "bold"), bg="#F0F8FF", fg="#0000CD") 
lng_label = tk.Label(window, text="Longitude: ", font=("Arial", 16, "bold"), bg="#F0F8FF", fg="#0000CD") 
link_label = tk.Label(window, text="Google Maps Link: ", font=("Arial", 16, "bold"), bg="#F0F8FF", fg="#0000CD") 
status_label = tk.Label(window, text="")

show_button = tk.Button(window, text="Show Location", command=show_location, font=("Arial", 14,"bold"), bg="#87CEFA", fg="#FFFFFF") 
emergency_button = tk.Button(window, text="Send Emergency", command=send_emergency, font=("Arial", 14,"bold" ), bg="#87CEFA", fg="#FFFFFF") 

lat_label.pack()
lng_label.pack()
link_label.pack()
show_button.pack()
emergency_button.pack()
status_label.pack()

window.mainloop()
