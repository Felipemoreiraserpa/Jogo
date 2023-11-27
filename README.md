# Jogo
import pygame
from pygame.locals import QUIT, KEYDOWN, K_a, K_d, K_w, K_s
from random import randint

pygame.init()

barulho_colisao = pygame.mixer.Sound('smw_coin.wav')

largura = 640
altura = 480
x = largura / 2
y = altura / 2

x_verde = randint(40, 600)
y_verde = randint(50, 430)

pontos = 0
fonte = pygame.font.SysFont('arial', 30, True, True)


tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption('jogo')
relogio = pygame.time.Clock()

game_over = False

while True:
    relogio.tick(60)
    tela.fill((0, 0, 0))
    mensagem = f'Pontos: {pontos}'
    texto_formatado = fonte.render(mensagem, True, (255, 0, 0))
    teclas = f'Use W, A, S, D para mover o vermelho'
    teclas_formatado = fonte.render(teclas, True, (255,255,255))
    instrucao = f'Coloque uma forma sobre a outra'
    instrucao_formatado = fonte.render(instrucao, True, (255, 255, 255))

    for event in pygame.event.get():
        if event.type == QUIT:
            pygame.quit()
            exit()

    if not game_over:
        if pygame.key.get_pressed()[K_a]:
            x = x - 20
        if pygame.key.get_pressed()[K_d]:
            x = x + 20
        if pygame.key.get_pressed()[K_w]:
            y = y - 20
        if pygame.key.get_pressed()[K_s]:
            y = y + 20

        # Verifica se o retângulo vermelho está dentro dos limites da tela
        if x < 0 or x > largura - 40 or y < 0 or y > altura - 50:
            game_over = True
            pygame.time.delay(250)  # Atraso antes de exibir a tela de Game Over

        ret_vermelho = pygame.draw.rect(tela, (255, 0, 0), (x, y, 40, 50))
        ret_verde = pygame.draw.rect(tela, (0, 255, 0), (x_verde, y_verde, 40, 50))

        if ret_vermelho.colliderect(ret_verde):
            x_verde = randint(40, 600)
            y_verde = randint(50, 430)
            pontos += 1
            barulho_colisao.play()

        tela.blit(texto_formatado, (10, 20))
        tela.blit(teclas_formatado, (60,400))
        tela.blit(instrucao_formatado, (80, 430))
    else:
        # Mensagem de "Game Over"
        mensagem_game_over = fonte.render('Game Over - Pressione R para reiniciar', True, (255, 0, 0))
        tela.blit(mensagem_game_over, (40, altura // 2))

        keys = pygame.key.get_pressed()
        if keys[pygame.K_r]:
            # Reinicia o jogo
            x = largura / 2
            y = altura / 2
            pontos = 0
            game_over = False

    pygame.display.update()

