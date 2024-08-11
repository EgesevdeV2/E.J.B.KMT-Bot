import discord
from discord.ext import commands
from PIL import Image, ImageDraw, ImageFont
import io
import aiohttp

intents = discord.Intents.default()
intents.members = True

client = commands.Bot(command_prefix="!", intents=intents)

@client.event
async def on_ready():
    print(f'Bot olarak giriş yapıldı: {client.user}')

@client.event
async def on_member_join(member):
    # Hoş geldin kartı şablonunun yolu
    template_path = 'welcome_card_template.png'
    
    # Kullanıcı fotoğrafını indir
    async with aiohttp.ClientSession() as session:
        async with session.get(member.avatar_url) as response:
            avatar_data = await response.read()
            avatar_image = Image.open(io.BytesIO(avatar_data))

            # Şablon görselini aç
            template = Image.open(template_path).convert("RGBA")
            template_width, template_height = template.size

            # Kullanıcı fotoğrafını şablon üzerine yerleştirin
            avatar_size = (150, 150)  # Kullanıcı fotoğrafının boyutu
            avatar_image = avatar_image.resize(avatar_size, Image.ANTIALIAS)
            template.paste(avatar_image, (50, 50), avatar_image)  # Fotoğrafı yerleştirin (x, y) konumunda
            
            # Yazı ekleyin
            draw = ImageDraw.Draw(template)
            font = ImageFont.load_default()  # Yani varsayılan yazı tipi
            text = f"Hoş geldin {member.name}!"
            text_position = (220, 100)  # Yazının konumu
            draw.text(text_position, text, font=font, fill=(255, 255, 255))

            # Görseli kaydedin
            output = io.BytesIO()
            template.save(output, format="PNG")
            output.seek(0)

            # Hoş geldin mesajını gönder
            channel = discord.utils.get(member.guild.text_channels, name='hoşgeldin')  # Kanal adı
            if channel:
                await channel.send(f"Hoş geldin {member.mention}!", file=discord.File(output, 'welcome_card.png'))

client.run('YOUR_BOT_TOKEN')  # Token'ınızı buraya yapıştırın
