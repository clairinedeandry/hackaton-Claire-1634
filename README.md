# Trash identify bot

## Deskripsi
Bot ini digunakan untuk mengidentifikasi jenis-jenis sampah.

##  bahan
model ai:Tensorflow
Library Discord.py
dataset gambar

import discord
from discord.ext import commands
from model import get_class

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

@bot.command()
async def hello(ctx):
    await ctx.send(f'Hello!! bot is ready, I am a bot {bot.user}!')

@bot.command()
async def heh(ctx, count_heh = 5):
    await ctx.send("he" * count_heh)

@bot.command()
async def check(ctx):
    # kode untuk bot menerima gambar
    if ctx.message.attachments: 
        for file in ctx.message.attachments: 
            file_name = file.filename 
            file_url = file.url
            await file.save(f'./{file.filename}')
            hasil = get_class(model_path=, labels_path='labels.txt', image_path=f'./{file.filename}')
            
            # kode untuk memproses gambar (ubah dengan melihat labels.txt)
            if hasil[0] == 'anorganik\n' and hasil[1] >= 0.65:
                await ctx.send('ini adalah sampah anorganik')
                await ctx.send('ini merupakan sampah anorganik, cukup berbahaya apabila dibiarkan')
                await ctx.send('Sampah ini t bisa di daur ulang,bisa dijadikan kerajinan,dll')
            elif hasil[0] == 'organik\n' and hasil[1] >= 0.65:
                await ctx.send('ini adalah sampah kaleng')
                await ctx.send('ini merupakan sampah anorganik, akan menimbulkan bau tak sedap dan mengotori lingkungan apabila dibiarkan')
                await ctx.send('tetapi sampah ini bisa dijadikan pupuk')
            elif hasil[0] == 'B3\n' and hasil[1] >= 0.65:
                await ctx.send('ini adalah sampah B3')
                await ctx.send('ini merupakan sampah B3, sampah ini berbahaya apabila dibiarkan karena ini adalah sampah limbah')
                await ctx.send('sampah ini tidak bisa di daur ulang')
           
            else:
                await ctx.send('GAMBAR MU KEMUNGKINAN: salah format/blur/corrupt')
                await ctx.send('KIRIM GAMBAR BARU!!!')
    else:
        await ctx.send('GAMBAR TIDAK VALID/GAADA >:/')


bot.run("MTMyNTc1MjU5NzMwMjQ4MDkyOQ.GcLuLF.AX_h2UJcNkJBtycTrhDovNRFDXELD1j4zOEUHE")


