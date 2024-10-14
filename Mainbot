import discord
from discord.ext import commands
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.options import Options

# Configuración de intents necesarios para obtener eventos de mensajes
intents = discord.Intents.default()
intents.message_content = True  # Esto permite que el bot lea los mensajes

# Configuración del bot
bot = commands.Bot(command_prefix="!", intents=intents)

# Función para obtener información del servidor
async def get_server_info():
    options = Options()
    options.headless = True  # Modo headless
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    
    print("Iniciando el WebDriver...")
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=options)

    try:
        # Navega a la página web
        print("Navegando al sitio web...")
        driver.get("https://servers.pandahut.net/")
        
        # Espera hasta que el nombre del servidor esté disponible
        
        server_name = ("Pandahut #20 [EU][Semi-Vanilla][10x][Old School]")

        # Espera hasta que el número de jugadores esté disponible
        print("Esperando que cargue el número de jugadores...")
        players_element = WebDriverWait(driver, 3).until(
            EC.presence_of_element_located((By.XPATH,"/html/body/div/div/div[3]/div/div[2]/div[20]/div[2]/div/ul[1]/li[6]/strong"))
        )
        num_players = players_element.text
        print(f"Number of Players: {num_players}")

        return server_name, num_players

    except Exception as e:
        print(f"Error al obtener la información: {str(e)}")
        return None, None

    finally:
        # Cerrar el WebDriver
        print("Cerrando el WebDriver...")
        driver.quit()

# Comando del bot para obtener información del servidor con !players
@bot.command()
async def players(ctx):

    await ctx.send("Getting the information about the server, please wait...")
    server_name, num_players = await get_server_info()

    if server_name and num_players:
        await ctx.send(f"Name of the server: {server_name}\nNumber of players: {num_players}")
    else:
        await ctx.send("No se pudo obtener la información del servidor.")

# Evento para cuando el bot se conecta correctamente
@bot.event
async def on_ready():
    print(f'Bot conectado como {bot.user}')

# Ejecutar el bot con el token
bot.run('MTI5NDYwNjc0NTg5MjgxOTAwNw.GU3N0k.sCxtI0x2c9i4IzJgCViN_5J1VBw02sjZoMML3w')
