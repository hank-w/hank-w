<<engine='python', engine.path='python3'>>=
# python code
@
# Note: Press L once to toggle invincibility, if you find
# your having a bit of trouble winning the levels

# Press the |> buttom in the top left corner to start :)

# Controls:
# L - toggle invincibility
# wasd / arrow keys - move spaceship
# j / space - shoot laser

# There are a total of 3 stages and an epic final boss!
# Have fun playing! :)

import simplegui, random, math

MENU = simplegui.load_sound("http://66.90.93.122/ost/megaman-3-original-soundtrack/ehqknkngbq/01%20-%20Title.mp3")
BOSS = simplegui.load_sound ("https://s1.vocaroo.com/media/download_temp/Vocaroo_s1vfUilOoSGJ.mp3")
GAME = simplegui.load_sound("http://66.90.93.122/ost/megaman-2-original-soundtrack/lwikzxsaci/18%20-%20Dr.%20Wily%27s%20Castle.mp3")
PLAYER_SHOOT_SND = simplegui.load_sound("https://s1.vocaroo.com/media/download_temp/Vocaroo_s1dOi4WYYywa.mp3")
ENEMY_SHOOT_SND = simplegui.load_sound("https://s1.vocaroo.com/media/download_temp/Vocaroo_s1qDMWaprwJU.mp3")
BOSS_LOCKONSHOOT_SND = simplegui.load_sound("https://s1.vocaroo.com/media/download_temp/Vocaroo_s13gOeBLx4jk.mp3")
BOSS_DIAGONALBULLET_SND = simplegui.load_sound("https://s0.vocaroo.com/media/download_temp/Vocaroo_s0zN0EjeIF2m.mp3")
ENEMY_DEATH_SND = simplegui.load_sound("https://s1.vocaroo.com/media/download_temp/Vocaroo_s1aFlS6iFrIk.mp3")
PLAYER_DEATH_SND = simplegui.load_sound("https://s1.vocaroo.com/media/download_temp/Vocaroo_s1kns3bV0bWw.mp3")
BOSS_HIT_SND = simplegui.load_sound("https://s0.vocaroo.com/media/download_temp/Vocaroo_s0YdfRadLh55.mp3")

TITLE_IMG = simplegui.load_image("https://i.imgur.com/dzgq1v6.png")
ENEMY1_IMG = simplegui.load_image("https://i.imgur.com/eYIqNH0.png")
ENEMY2_IMG = simplegui.load_image("https://i.imgur.com/5JpmASf.png")
ENEMY3_IMG = simplegui.load_image("https://i.imgur.com/XYCChOS.png")
BOSS_IMG = simplegui.load_image("https://i.imgur.com/q9p6xXs.png")
BOSS_HIT_IMG = simplegui.load_image("https://i.imgur.com/E8ONXLM.png")
BOSS_DIAGONALBULLET_IMG = simplegui.load_image("https://i.imgur.com/lCmd6Dp.png")
BOSS_LOCKONBULLET_IMG = simplegui.load_image("https://i.imgur.com/2aFGLK5.png")
PLAYER_IMG = simplegui.load_image("https://i.imgur.com/arFJBbI.png")
BACKGROUND_IMG = simplegui.load_image("https://i.imgur.com/0K2iWui.png")
BULLET_IMG = simplegui.load_image("https://i.imgur.com/hUShQgX.png")
ENEMY_BULLET_IMG = simplegui.load_image("https://i.imgur.com/K573fAX.png")
EXPLOSION_IMG = simplegui.load_image("https://i.imgur.com/6VYEzW0.png")

WIDTH = 800
HEIGHT = 800
TITLE_WIDTH = 304
TITLE_HEIGHT  = 32
TITLE_SIZE = (TITLE_WIDTH*1.5,TITLE_HEIGHT*1.5)
PLAYER_WIDTH = 400/8
PLAYER_HEIGHT= 42
PLAYER_SIZE = (PLAYER_WIDTH*1.5,PLAYER_HEIGHT*1.5)
BACKGROUND_WIDTH = 405
BACKGROUND_HEIGHT = 810
BULLET_WIDTH = 64/8
BULLET_HEIGHT = 14
BULLET_SIZE = (BULLET_WIDTH*1.5,BULLET_HEIGHT*1.5)
LOCKONBULLET_WIDTH = 60/3
LOCKONBULLET_HEIGHT = 20
LOCKONBULLET_SIZE = (LOCKONBULLET_WIDTH,LOCKONBULLET_HEIGHT)
DIAGONALBULLET_WIDTH = 132 / 6
DIAGONALBULLET_HEIGHT = 20
DIAGONALBULLET_SIZE = (DIAGONALBULLET_WIDTH,DIAGONALBULLET_HEIGHT)
ENEMY1_WIDTH = 448 / 8
ENEMY1_HEIGHT = 49
ENEMY1_SIZE = [ENEMY1_WIDTH,ENEMY1_HEIGHT]
ENEMY2_WIDTH = 480 / 8
ENEMY2_HEIGHT = 64
ENEMY2_SIZE = [ENEMY2_WIDTH,ENEMY2_HEIGHT]
ENEMY3_WIDTH = 512 / 8
ENEMY3_HEIGHT = 69
ENEMY3_SIZE = [ENEMY3_WIDTH*0.9,ENEMY3_HEIGHT*0.9]
BOSS_WIDTH = 2112 / 4
BOSS_HEIGHT = 176
BOSS_SIZE = [BOSS_WIDTH,BOSS_HEIGHT]
EXPLOSION_WIDTH = 290 / 5
EXPLOSION_HEIGHT = 58
EXPLOSION_SIZE = [EXPLOSION_WIDTH*1.5,EXPLOSION_HEIGHT*1.5]

enemy1_list = []
enemy2_list = []
enemy3_list = []
boss_list = []
bullet_list = []
enemy2_bullet_list = []
boss_bullet_list = []
boss_lockonbullet_list = []
boss_diagonalbullet_list = []
explosion_list = []
player_explosion_list = []

background_pos = [WIDTH/2,HEIGHT/2]
scroll_speed = 1
cycles = 0
hanke = 0
score = 0
stage = ""
highscore = 0

enemy2_shoot_hanker = 0
boss_shoot_hanker = 0
shoot_cooldown = 0
boss_hanker= 0
boss_respawn = False
boss_respawn_hanker = 0
boss_escape = False
boss_health = 0
boss_damage = False
boss_damage_hanker = 0
boss_movement_hanker = 0
boss_fight = False
boss_left = 0
boss_right = 0
bullet_x = 49
diagonalbullet_x = 112
enemy2_respawn = False
enemy2_respawn_hanker = 0
enemy3_respawn = False
enemy3_respawn_hanker = 0

up = False
down = False
right = False
left = False

game_started = False

player_is_dead = False
explosion_hanker = 0

text_hanker = 0

fx_volume = 0.5
music_volume = 0.5

god_mode = False


# This function checks if an object collides with
# another in terms of radius.
def dist(pos1, pos2):
    a = pos2[1]-pos1[1]
    b = pos2[0]-pos1[0]
    dis = math.sqrt(a**2+b**2)
    return dis


# This functions resets everything when the game is
# started or restarted.
def new_game():
    global player,hanke,boss_hanker,boss_left,boss_right,boss_list,boss_damage_hanker,boss_health, boss_damage,boss_escape, boss_lockonbullet_list, boss_fight, bullet_x,diagonalbullet_x,boss_diagonalbullet_list,boss_shoot_hanker, boss_movement_hanker, text_hanker, explosion_hanker, player_explosion_list, player_is_dead, game_started, bullet_list, enemy2_bullet_list, boss_bullet_list, enemy1_list, enemy2_list, enemy3_list, score, left, down, right, up
    player = Character(PLAYER_IMG,
                      [WIDTH/2,HEIGHT/2],
                      [0,0],30)
    boss_hanker = 0
    boss_list = []
    boss_left = 0
    boss_right = 0
    boss_fight = False
    boss_bullet_list = []
    boss_lockonbullet_list = []
    boss_diagonalbullet_list = []
    diagonalbullet_x = 112
    bullet_x = 49
    boss_shoot_hanker = 0
    boss_movement_hanker = 0
    boss_escape = False
    boss_health = 0
    boss_damage_hanker = 0
    boss_damage = False
    
    up = False
    down = False
    right = False
    left = False
    game_started = False
    player_is_dead = False
    
    bullet_list = []
    enemy2_bullet_list = []
    enemy1_list = []
    enemy2_list = []
    enemy3_list = []
    explosion_list = []
    player_explosion_list = []
    
    stage_hanker = 0
    explosion_hanker = 0
    text_hanker = 0
    hanke = 0
    score = 0

    
# This is the class that creates the default bullets for
# enemies and the player
class Bullet:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.hanke = 0
        self.image = image
        self.radius = radius
        
    def draw(self, canvas):
        tile_center = [BULLET_WIDTH/2 + self.hanke%8*BULLET_WIDTH,
                       BULLET_HEIGHT/2]
        canvas.draw_image(self.image,
                     tile_center,
                     [BULLET_WIDTH,BULLET_HEIGHT],
                     self.pos,
                     BULLET_SIZE)
        #canvas.draw_circle(self.pos,self.radius,1,"Red")

    def update(self):
        for i in range(2):
            self.pos[i] += self.vel[i]  
        if animated and cycles%5 == 0:
            self.hanke += 1 

            
# This is the class that creates the boss bullets that
# lock on to the player
class LockOnBullet:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.hanke = 0
        self.image = image
        self.radius = radius
        
    def draw(self, canvas):
        tile_center = [LOCKONBULLET_WIDTH/2 + self.hanke%3*LOCKONBULLET_WIDTH,
                       LOCKONBULLET_HEIGHT/2]
        canvas.draw_image(self.image,
                     tile_center,
                     [LOCKONBULLET_WIDTH,LOCKONBULLET_HEIGHT],
                     self.pos,
                     LOCKONBULLET_SIZE)
        #canvas.draw_circle(self.pos,self.radius,1,"Green")

    def update(self):
        for i in range(2):
            self.pos[i] += self.vel[i]              
        if animated and cycles%5 == 0:
            self.hanke += 1            
        if player_is_dead == False:
            if boss_fight:
# This line of code checks where the player is in respect
# to the bullet and changes velocity so that the bullet
# follows the player.
                if player.pos[1] > self.pos[1] and self.vel[1] < 5 and self.hanke < 30:
                    self.vel[1] += 0.5
                elif player.pos[1] < self.pos[1] and self.vel[1] > -5 and self.hanke < 30:
                    self.vel[1] -= 0.5                         
                if player.pos[0] > self.pos[0] and self.vel[0] < 5 and self.hanke < 30:
                    self.vel[0] += 0.5
                elif player.pos[0] < self.pos[0] and self.vel[0] > -5 and self.hanke < 30:
                    self.vel[0] -= 0.5
            else:
                self.vel[1] = 5
                
                
# This is the class that creates the boss bullets that
# shoot diagonally
class DiagonalBullet:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.hanke = 0
        self.image = image
        self.radius = radius
        
    def draw(self, canvas):
        tile_center = [DIAGONALBULLET_WIDTH/2 + self.hanke%6*DIAGONALBULLET_WIDTH,
                       DIAGONALBULLET_HEIGHT/2]
        canvas.draw_image(self.image,
                     tile_center,
                     [DIAGONALBULLET_WIDTH,DIAGONALBULLET_HEIGHT],
                     self.pos,
                     DIAGONALBULLET_SIZE)
        #canvas.draw_circle(self.pos,self.radius,1,"Green")

    def update(self):
        for i in range(2):
            self.pos[i] += self.vel[i]              
        if animated and cycles%5 == 0:
            self.hanke += 1   
                        
                
# This is the class for creating explosions where
# enemies or the player dies
class Explosion:
    def __init__(self, image, position, velocity):
        self.pos = position
        self.vel = velocity
        self.image = image
        self.hanke = 0
        self.size = EXPLOSION_SIZE
    def draw(self, canvas):
        tile_center = [EXPLOSION_WIDTH/2 + self.hanke%5*EXPLOSION_WIDTH,
                       EXPLOSION_HEIGHT/2]
        canvas.draw_image(self.image,
                         tile_center,
                         [EXPLOSION_WIDTH,EXPLOSION_HEIGHT],
                         self.pos,
                         self.size)
        #canvas.draw_circle(self.pos,self.radius,1,"Red")
        
    def update(self):
        for i in range(2):
            self.pos[i] += self.vel[i]            
        if animated and cycles%5 == 0:
            self.hanke += 1   

            
# This is the class for the basic enemy. It moves in a
# straight line.
class Enemy1:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.image = image
        self.hanke = 0
        self.radius = radius
        self.size = ENEMY1_SIZE
    def draw(self, canvas):
        tile_center = [ENEMY1_WIDTH/2 + self.hanke%8*ENEMY1_WIDTH,
                       ENEMY1_HEIGHT/2]
        canvas.draw_image(self.image,
                         tile_center,
                         [ENEMY1_WIDTH,ENEMY1_HEIGHT],
                         self.pos,
                         self.size)
        #canvas.draw_circle(self.pos,self.radius,1,"Red")
        
    def update(self):
        for i in range(2):
            self.pos[i] += self.vel[i]            
        if animated and cycles%5 == 0:
            self.hanke += 1 
            
    def has_collided(self, other):
        return self.radius+other.radius>=dist(self.pos,other.pos)        
    
    
# This is the class for the second enemy. It follows the
# player horizontally and shoots
class Enemy2:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.image = image
        self.hanke = 0
        self.radius = radius
        self.size = ENEMY2_SIZE
    def draw(self, canvas):
        tile_center = [ENEMY2_WIDTH/2 + self.hanke%8*ENEMY2_WIDTH,
                       ENEMY2_HEIGHT/2]
        canvas.draw_image(self.image,
                         tile_center,
                         [ENEMY2_WIDTH,ENEMY2_HEIGHT],
                         self.pos,
                         self.size)
        #canvas.draw_circle(self.pos,self.radius,1,"Red")
        
    def update(self):
        global enemy2_shoot_hanker
        for i in range(2):
            self.pos[i] += self.vel[i]            
        if animated and cycles%5 == 0:
            self.hanke += 1
# If the player is dead, enemies stop moving and shooting      
        if player_is_dead == False:
            if boss_fight:
                self.vel[1] = -5
            else:
                if self.pos[1] > 30:
                    self.vel[1] = 0
# This is the hanker that determines how fast it shoots
                enemy2_shoot_hanker += 1    
# This line makes the enemy follow the player
# horizontally
                if player.pos[0] > self.pos[0]+10:
                    self.vel[0] = 5
                elif player.pos[0] < self.pos[0]-10:
                    self.vel[0] = -5
                else:
                    self.vel[0] = 0
        else:
            self.vel = [0,0]

# This is the function for shooting once the enemy gets
# in position
    def shoot(self):
        if player_is_dead == False:    
            if self.pos[1] > 30:
                ENEMY_SHOOT_SND.rewind()
                ENEMY_SHOOT_SND.play()
                enemy2_bullet = Bullet(ENEMY_BULLET_IMG,[self.pos[0],self.pos[1]-ENEMY2_SIZE[1]/2+70],[0,20], 4)
                enemy2_bullet_list.append(enemy2_bullet)
        
    def has_collided(self, other):
        return self.radius+other.radius>=dist(self.pos,other.pos)        

    
# This is the class for the third enemy. It follows the
# player of a duration of hanke
class Enemy3:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.image = image
        self.hanke = 0
        self.radius = radius
        self.size = ENEMY3_SIZE
    def draw(self, canvas):
        tile_center = [ENEMY3_WIDTH/2 + self.hanke%8*ENEMY3_WIDTH,
                       ENEMY3_HEIGHT/2]
        canvas.draw_image(self.image,
                         tile_center,
                         [ENEMY3_WIDTH,ENEMY3_HEIGHT],
                         self.pos,
                         self.size)
        #canvas.draw_circle(self.pos,self.radius,1,"Red")
        
    def update(self):
        global enemy3_lock_on
        for i in range(2):
            self.pos[i] += self.vel[i]            
        if animated and cycles%5 == 0:
            self.hanke += 1
        if player_is_dead == False:
            if boss_fight:
                self.vel[1] = 5                
            else:
                if player.pos[1] > self.pos[1] and self.vel[1] < 5 and self.hanke < 30:
                    self.vel[1] += 0.5
                elif player.pos[1] < self.pos[1] and self.vel[1] > -5 and self.hanke < 30:
                    self.vel[1] -= 0.5                     
                if player.pos[0] > self.pos[0] and self.vel[0] < 5 and self.hanke < 30:
                    self.vel[0] += 0.5
                elif player.pos[0] < self.pos[0] and self.vel[0] > -5 and self.hanke < 30:
                    self.vel[0] -= 0.5
        else:
            self.vel = [0,0]
        
    def has_collided(self, other):
        return self.radius+other.radius>=dist(self.pos,other.pos) 

    
# This is the class for the boss. He will move right and
# left on the screen while firing bullets, lock on
# bullets, and diagonal bullets. The player needs to hit
# him 20 hankes where he then flies away and come again
# later
class Boss:
    def __init__(self, image, position, velocity, radius):
        self.pos = position
        self.vel = velocity
        self.image = image
        self.hanke = 0
        self.radius = radius
        self.size = BOSS_SIZE
    def draw(self, canvas):
        tile_center = [BOSS_WIDTH/2 + self.hanke%4*BOSS_WIDTH,
                       BOSS_HEIGHT/2]
        canvas.draw_image(self.image,
                         tile_center,
                         [BOSS_WIDTH,BOSS_HEIGHT],
                         self.pos,
                         self.size)
        #canvas.draw_polygon([(self.pos[0]-250,self.pos[1]+60),(self.pos[0]-250,self.pos[1]-60),(self.pos[0]+250,self.pos[1]-60),(self.pos[0]+250,self.pos[1]+60)],2,"#fa471f")
        
    def update(self):
        global boss_damage, boss_damage_hanker, boss_shoot_hanker, boss_movement_hanker, boss_left, boss_right
        for i in range(2):
            self.pos[i] += self.vel[i]            
        if animated and cycles%5 == 0:
            self.hanke += 1           
# For this line, when the boss takes damage, the boss
# images is replaced with a "damaged" version for a
# short period
        if boss_damage and boss_damage_hanker < 10:
            boss_damage_hanker += 1
        else:
            boss_damage = False
            boss_damage_hanker = 0             
        if boss_damage: 
            self.image = BOSS_HIT_IMG
        else:
            self.image = BOSS_IMG              
        if player_is_dead == False:
            if boss_escape:
                self.vel[1] = -5
            elif self.pos[1] == 30:
                self.vel[1] = 0
                boss_shoot_hanker += 1                
# This line of code makes the Boss move left or right
# on the screen. When he's on the edge of the screen,
# he will move back towards the middle. The direction
# the boss moves is random but boss_left and boss_right
# prevent it from moving in the same direction more than
# twice to ensure variety
            if boss_escape or self.pos[1] < 30:
                self.vel[0] = 0
            elif self.pos[0] == 400:
                self.vel[0] = 0
                if boss_movement_hanker < 60:
                    boss_movement_hanker += 1
                elif boss_left == 2:
                    self.vel[0] = 5
                    boss_movement_hanker = 0
                elif boss_right == 2:
                    self.vel[0] = -5
                    boss_movement_hanker = 0
                else:
                    self.vel[0] = random.choice([5,-5])
                    boss_movement_hanker = 0
                boss_spawned = True
            elif self.pos[0] < 100:
                self.vel[0] = 0                
                if boss_movement_hanker == 60:
                    boss_left += 1
                boss_right = 0               
                if boss_movement_hanker < 60:
                    boss_movement_hanker += 1
                else:   
                    self.vel[0] = 5
                    boss_movement_hanker = 0
            elif self.pos[0] > 700:
                self.vel[0] = 0               
                if boss_movement_hanker == 60:
                    boss_right += 1
                boss_left = 0                
                if boss_movement_hanker < 60:
                    boss_movement_hanker += 1
                else:   
                    self.vel[0] = -5
                    boss_movement_hanker = 0
        else:
            self.vel = [0,0]
            
    def shoot(self): 
        global bullet_x
        if self.pos[1] >= 30 and self.vel[0] != 0 and player_is_dead == False:
            ENEMY_SHOOT_SND.rewind()
            ENEMY_SHOOT_SND.play()
# This line of codes makes it so everyhanke the boss
# shoots, the bullet spawns alternates between 3
# different pairs of cannons
            if bullet_x == 49:
                bullet_x = 59
                bullet_y = 150
            elif bullet_x == 59:
                bullet_x = 69
                bullet_y = 145
            elif bullet_x == 69:
                bullet_x = 49
                bullet_y = 145                
            boss_bullet1 = Bullet(ENEMY_BULLET_IMG,[self.pos[0]-bullet_x,self.pos[1]-BOSS_SIZE[1]/2+bullet_y],[0,20], 4)
            boss_bullet2 = Bullet(ENEMY_BULLET_IMG,[self.pos[0]+bullet_x,self.pos[1]-BOSS_SIZE[1]/2+bullet_y],[0,20], 4)
            boss_bullet_list.append(boss_bullet1)
            boss_bullet_list.append(boss_bullet2)
            
    def shootlockon(self):
            if self.pos[1] >= 30 and player_is_dead == False:
                BOSS_LOCKONSHOOT_SND.rewind()
                BOSS_LOCKONSHOOT_SND.play()
                boss_lockonbullet1 = LockOnBullet(BOSS_LOCKONBULLET_IMG,[self.pos[0]-175,self.pos[1]-BOSS_SIZE[1]/2+160],[5,5], 7)
                boss_lockonbullet2 = LockOnBullet(BOSS_LOCKONBULLET_IMG,[self.pos[0]+175,self.pos[1]-BOSS_SIZE[1]/2+160],[-5,5], 7)
                boss_lockonbullet_list.append(boss_lockonbullet1)
                boss_lockonbullet_list.append(boss_lockonbullet2)
                
    def shootdiagonal(self):
        global diagonalbullet_x
        if self.pos[1] >= 30 and self.vel[0] == 0 and player_is_dead == False: 
            BOSS_DIAGONALBULLET_SND.rewind()
            BOSS_DIAGONALBULLET_SND.play()           
# Similar to the bullet_x and bullet_y variable but
# depending on where the bullet spawns, the horizontal
# velocity is different
            if diagonalbullet_x == 112:
                diagonalbullet_x = 17
                diagonalbullet_y = 180
            elif diagonalbullet_x == 17:
                diagonalbullet_x = 112
                diagonalbullet_y = 155
                
            if self.pos[0] == 400:
                diagonalbullet_x_vel = 5
            else:
                diagonalbullet_x_vel = 10
            boss_diagonalbullet1 = DiagonalBullet(BOSS_DIAGONALBULLET_IMG,[self.pos[0]-diagonalbullet_x,self.pos[1]-BOSS_SIZE[1]/2+diagonalbullet_y],[-diagonalbullet_x_vel,10], 6)
            boss_diagonalbullet2 = DiagonalBullet(BOSS_DIAGONALBULLET_IMG,[self.pos[0]+diagonalbullet_x,self.pos[1]-BOSS_SIZE[1]/2+diagonalbullet_y],[diagonalbullet_x_vel,10], 6)
            boss_diagonalbullet_list.append(boss_diagonalbullet1)
            boss_diagonalbullet_list.append(boss_diagonalbullet2)

    def has_collided(self, other):
# This line of code is similar to the collision function
# but checks in a square area rather than radius
        cond1 = math.fabs(other.pos[0] - self.pos[0]) < (other.radius+500/2)
        cond2 = math.fabs(other.pos[1] - self.pos[1]) < (other.radius+120/2)
        if cond1 and cond2:
            return True 

# This function resets all boss variables, gives the
# player score points and allows the stage to progress
    def death(self):
        global boss_health,boss_fight,boss_escape,boss_movement_hanker,boss_shoot_hanker,boss_hanker,score,hanke
        if boss_health == 20:
            boss_escape = True
            boss_health = 0
            boss_fight = False
            boss_movement_hanker = 0
            boss_shoot_hanker = 0
            boss_hanker = 0
            score += 10000
            hanke+= 1

            
# This is the class for the character. It can move and
# shoot
class Character:
    def __init__(self,image,position,velocity,radius):
        self.image = image
        self.pos = position
        self.vel = velocity
        self.hanke = 0
        self.radius = radius

        
    def draw(self, canvas): 
        tile_center = [PLAYER_WIDTH/2 + self.hanke%8*PLAYER_WIDTH,
                       PLAYER_HEIGHT/2]
        canvas.draw_image(self.image,
                     tile_center,
                     [PLAYER_WIDTH,PLAYER_HEIGHT],
                     self.pos,
                     PLAYER_SIZE)
        #canvas.draw_circle(self.pos,self.radius,1,"Red")
    
    def update(self):
        global shoot_cooldown
        if animated and cycles%5 == 0:
            self.hanke += 1
        self.hanke %= 8
        self.pos[0] += self.vel[0]
        self.pos[1] += self.vel[1]          
# This line stops the player from going off screen and
# gives the ability to shift to the opposite side of the
# of the screen
        if self.pos[0] < 0:
            self.pos[0] = 800            
        if self.pos[0] > 800:
            self.pos[0] = 0              
        if self.pos[1] > 770:
            self.pos[1] = 770            
        if self.pos[1] < 30:
            self.pos[1] = 30             
# This code is used to move the player. It allows
# smoother movement compared to placing the velocity in
# the key handler and adjusts velocity when pressing both
# a vertical and horizontal key as to prevent an unwanted
# speed boost
        if up and left:
            self.vel = [-7.5,-7.5]
        elif down and left:
            self.vel = [-7.5,7.5]
        elif left:
            self.vel[0] = -10
        elif right:
            pass
        else:
            self.vel[0] = 0             
        if up and right:
            self.vel = [7.5,-7.5]
        elif down and right:
            self.vel = [7.5,7.5]
        elif right:
            self.vel[0] = 10
        elif left:
            pass
        else:
            self.vel[0] = 0
            
        if up and left:
            pass
        elif up and right:
            pass
        elif up:
            self.vel[1] = -10
        elif down:
            pass
        else:
            self.vel[1] = 0            
        if down and right:
            pass
        if down and left:
            pass
        elif down:
            self.vel[1] = 10
        elif up:
            pass
        else:
            self.vel[1] = 0  
        
        if shoot_cooldown > 0:
            shoot_cooldown -= 1
# This function is for the player to shoot. After each
# shot there is a cooldown before another shot can be
# done
    def shoot(self):
        global shoot_cooldown
        bullet = Bullet(BULLET_IMG,[self.pos[0],self.pos[1]-PLAYER_SIZE[1]/2-5],[0,-20], 5)
        if shoot_cooldown == 0:
            PLAYER_SHOOT_SND.rewind()
            PLAYER_SHOOT_SND.play()
            bullet_list.append(bullet)
            shoot_cooldown = 20
                        
    def has_collided(self, other):
        return self.radius+other.radius>=dist(self.pos,other.pos)      

    
# Key handlers that start the game, make the player move,
# lower sound, and etc. when keys are inputted
def key_up(key):
    global up,down,left,right
    if simplegui.KEY_MAP['w'] == key or simplegui.KEY_MAP['up'] == key:
        if game_started:
            up = False
    if simplegui.KEY_MAP['s'] == key or simplegui.KEY_MAP['down'] == key:
        if game_started:
            down = False
    if simplegui.KEY_MAP['d'] == key or simplegui.KEY_MAP['right'] == key:
        if game_started:
            right = False
    if simplegui.KEY_MAP['a'] == key or simplegui.KEY_MAP['left'] == key:
        if game_started:
            left = False
            
            
def key_down(key):
    global up,down,left,right,game_started, god_mode, music_volume, fx_volume
    if simplegui.KEY_MAP['w'] == key or simplegui.KEY_MAP['up'] == key:
        if game_started:
            up = True
            down = False
    if simplegui.KEY_MAP['s'] == key or simplegui.KEY_MAP['down'] == key:
        if game_started:
            down = True
            up = False
    if simplegui.KEY_MAP['a'] == key or simplegui.KEY_MAP['left'] == key:
        if game_started:
            left = True
            right = False
    if simplegui.KEY_MAP['d'] == key or simplegui.KEY_MAP['right'] == key:
        if game_started:
            right = True
            left = False
    if simplegui.KEY_MAP['space'] == key or simplegui.KEY_MAP['j'] == key:
        if game_started:
            player.shoot()
        if player_is_dead:
            new_game()
        else:
            game_started = True
# These handlers control the volume of sounds
    if simplegui.KEY_MAP['p'] == key:
        if fx_volume < 0.9:
            fx_volume += 0.1
    if simplegui.KEY_MAP['o'] == key:
        if fx_volume > 0.2:
            fx_volume -= 0.1
    if simplegui.KEY_MAP['i'] == key:
        if music_volume < 0.9:
            music_volume += 0.1
    if simplegui.KEY_MAP['u'] == key:
        if music_volume > 0.2:
            music_volume -= 0.1
# This handler toggles invincibility mode
    if simplegui.KEY_MAP['l'] == key:
        if god_mode == False:
            god_mode = True
        else:
            god_mode = False
    
    
def draw(canvas):
    global bullet, hanke,explosion_hanker, player_explosion_list,boss_hanker, boss_fight, boss_damage, boss_movement_hanker, boss_shoot_hanker,boss_bullet, boss_health, boss_escape, stage_hanker, text_hanker, stage, bullet,enemy2_bullet, player_is_dead, cycles, highscore, score, enemy2_respawn_hanker, enemy2_respawn, enemy3_respawn_hanker, enemy3_respawn
    cycles += 1
# These lines update the volume of the sound effects
# and music
    MENU.set_volume(music_volume)
    GAME.set_volume(music_volume)
    BOSS.set_volume(music_volume)
    PLAYER_SHOOT_SND.set_volume(fx_volume)
    ENEMY_SHOOT_SND.set_volume(fx_volume)
    BOSS_LOCKONSHOOT_SND.set_volume(fx_volume)
    BOSS_DIAGONALBULLET_SND.set_volume(fx_volume)
    ENEMY_DEATH_SND.set_volume(fx_volume)
    PLAYER_DEATH_SND.set_volume(fx_volume)
    BOSS_HIT_SND.set_volume(fx_volume)
    
# This line draws the scrolling background
    canvas.draw_image(BACKGROUND_IMG,
                     (BACKGROUND_WIDTH/2,BACKGROUND_HEIGHT/2),
                     (BACKGROUND_WIDTH,BACKGROUND_HEIGHT),
                     background_pos,
                     (WIDTH,HEIGHT*2))
    background_pos[1] = (background_pos[1] + scroll_speed)%(HEIGHT)
#when player is alive, the player is drawn but if
#the player is dead, spawns an explosion on the player
#and the player is not drawn
    if player_is_dead == False:
        player.draw(canvas)
        player.update()    
    elif player_is_dead:
        player_death = Explosion(EXPLOSION_IMG, [player.pos[0],player.pos[1]-10],[0,0])
        if player_explosion_list != []:
            explosion_hanker += 1
        if player_explosion_list == [] and explosion_hanker < 21:
            player_explosion_list.append(player_death)
        if explosion_hanker > 20:
            player_explosion_list = []
            
    if game_started:
        MENU.rewind()
# When there isn't a boss fight, the hanker goes up and 
#determines when next stage happens
        if boss_fight:
            BOSS.play()
            GAME.rewind()
        else:
            GAME.play()
            BOSS.rewind()
            hanke += 1            
# If the player is alive, the score increases over hanke
        if player_is_dead == False:    
            score += 1            
# Sets the high score for the session and replaces it if
# the current score exceeds it
        if score > highscore:
            highscore = score
                       
# These lines of code control the enemy spawns, spawn
# position, and initial velocity
        if boss_fight == False:
            if hanke % 30 == 0:
                x1 = random.randrange(WIDTH)
                y1 = 0
                pos1 = [x1,y1]
                vx1 = 0                
                if hanke < 360:
                    vy1 = 5
                else:
                    vy1 = 10
                vel1 = [vx1,vy1]
                enemy1 = Enemy1(ENEMY1_IMG,pos1,vel1,30)                
                if hanke < 360:
                    if hanke %60:
                        enemy1_list.append(enemy1)
                else: 
                    enemy1_list.append(enemy1)
                    
# For this line, when one of the shooting enemies is on 
# screen, it won't spawn anymore of it and after the 
# enemy dies, a hanker starts before it respawns.
# every enemy type after the first spawns once the hanke
# has reached a certain point
            if enemy2_list == [] and hanke > 2520 and player_is_dead == False:
                enemy2_respawn = True
            else:
                enemy2_respawn = False                
            if enemy2_respawn:
                enemy2_respawn_hanker += 1                 
            if enemy2_list == [] and hanke > 2520 and enemy2_respawn_hanker %180 == 0 and player_is_dead == False and boss_fight == False:
                x2 = random.randrange(WIDTH)
                y2 = 0
                pos2 = [x2,y2]
                vx2 = 0
                vy2 = 5
                vel2 = [vx2,vy2]
                enemy2 = Enemy2(ENEMY2_IMG,pos2,vel2,30)
                enemy2_list.append(enemy2)                 
            if hanke > 4680 and player_is_dead == False:
                enemy3_respawn = True
            else:
                enemy3_respawn = False               
            if enemy3_respawn:
                enemy3_respawn_hanker += 1
                
            if hanke > 4680 and enemy3_respawn_hanker %180 == 0 and player_is_dead == False and boss_fight == False:       
                if player.pos[0] > 400:
                    x3 = 0
                else:
                    x3 = 800
                y3 = random.randrange(300,450)
                pos3 = [x3,y3]
                vx3 = 0
                vy3 = 0
                vel3 = [vx3,vy3]
                enemy3 = Enemy3(ENEMY3_IMG,pos3,vel3,30)
                enemy3_list.append(enemy3)
                
        if boss_list == []:
            boss_escape = False
        if boss_list == [] and boss_hanker > 540 and boss_fight:
            x2 = WIDTH/2
            y2 = -90
            pos2 = [x2,y2]
            vx2 = 0
            vy2 = 5
            vel2 = [vx2,vy2]
            boss = Boss(BOSS_IMG,pos2,vel2,90)
            boss_list.append(boss)
            
# This set of code is for all enemy related actions such
# as collisions with the player, deleting when moving
# off screen, shooting, drawing, and updating
        for enemy1 in enemy1_list:
            enemy1.draw(canvas)
            enemy1.update()
            if enemy1.pos[1]>830:
                enemy1_list.remove(enemy1)
            if enemy1.has_collided(player):
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:    
                    player_is_dead = True
                    
        for enemy2 in enemy2_list:
            enemy2.draw(canvas)
            enemy2.update()
            if enemy2_shoot_hanker % 60 == 0:
                enemy2.shoot()
            if enemy2.pos[1]<-30:
                enemy2_list.remove(enemy2)
            if enemy2.has_collided(player):
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:    
                    player_is_dead = True
                    
        for enemy3 in enemy3_list:
            enemy3.draw(canvas)
            enemy3.update()
            if enemy3.hanke > 30:
                if enemy3.pos[1]>820 or enemy3.pos[1]<-20 or enemy3.pos[0]>820 or enemy3.pos[0]<-20:
                    enemy3_list.remove(enemy3)
            if enemy3.has_collided(player):
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:    
                    player_is_dead = True       
                
        for enemy2_bullet in enemy2_bullet_list:
            enemy2_bullet.draw(canvas)
            enemy2_bullet.update()
            if enemy2_bullet.pos[1]>830:
                enemy2_bullet_list.remove(enemy2_bullet)
            if player.has_collided(enemy2_bullet):
                enemy2_bullet_list.remove(enemy2_bullet)
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:    
                    player_is_dead = True
                    
        for boss in boss_list:
            boss.draw(canvas)
            boss.update()
            if boss.pos[1] < -90 and boss_escape:
                boss_list.remove(boss)
            if boss_escape == False:    
                if boss_shoot_hanker %10 == 0:
                    boss.shoot()
                if boss_shoot_hanker % 120 == 0:
                    boss.shootlockon()
                if boss_shoot_hanker % 10 == 0:
                    boss.shootdiagonal()   
            if boss.has_collided(player):
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:
                    player_is_dead = True
                    
        for boss_bullet in boss_bullet_list:
            boss_bullet.draw(canvas)
            boss_bullet.update()
            if boss_bullet.pos[1]>830:
                boss_bullet_list.remove(boss_bullet)
            if player.has_collided(boss_bullet):
                boss_bullet_list.remove(boss_bullet)
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:    
                    player_is_dead = True
                    
        for boss_diagonalbullet in boss_diagonalbullet_list:
            boss_diagonalbullet.draw(canvas)
            boss_diagonalbullet.update()
            if boss_diagonalbullet.pos[1]>802 or boss_diagonalbullet.pos[1]<-2 or boss_diagonalbullet.pos[0]>802 or boss_diagonalbullet.pos[0]<-2:
                boss_diagonalbullet_list.remove(boss_diagonalbullet)
            if player.has_collided(boss_diagonalbullet):
                boss_diagonalbullet_list.remove(boss_diagonalbullet)
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:
                    player_is_dead = True
                    
        for boss_lockonbullet in boss_lockonbullet_list:
            boss_lockonbullet.draw(canvas)
            boss_lockonbullet.update()
            if boss_lockonbullet.hanke > 30:
                if boss_lockonbullet.pos[1]>802 or boss_lockonbullet.pos[1]<-2 or boss_lockonbullet.pos[0]>802 or boss_lockonbullet.pos[0]<-2:
                    boss_lockonbullet_list.remove(boss_lockonbullet)
            if player.has_collided(boss_lockonbullet):
                boss_lockonbullet_list.remove(boss_lockonbullet)
                if player_is_dead == False:
                    PLAYER_DEATH_SND.rewind()
                    PLAYER_DEATH_SND.play()
                if god_mode == False:    
                    player_is_dead = True
                    
# This set of code is used for things related to the
# player's bullets including collisions with enemies
# off screen deletions, and what happens to enemies
# and bullet when they collide
        for bullet in bullet_list:
            bullet.draw(canvas)
            bullet.update()
            if bullet.pos[1]<-30:
                bullet_list.remove(bullet)
            for enemy1 in enemy1_list:
                    if enemy1.has_collided(bullet):
                        score += 200
                        death = Explosion(EXPLOSION_IMG, [enemy1.pos[0],enemy1.pos[1]-10],[enemy1.vel[0],enemy1.vel[1]])
                        explosion_list.append(death)
                        if bullet in bullet_list:
                            bullet_list.remove(bullet)
                        if enemy1 in enemy1_list:
                            ENEMY_DEATH_SND.rewind()
                            ENEMY_DEATH_SND.play()
                            enemy1_list.remove(enemy1)
                        for explosion in explosion_list:
                            death.draw(canvas)
                            death.update()
                            if explosion in explosion_list:
                                explosion_list.remove(death)
            for enemy2 in enemy2_list:
                    if enemy2.has_collided(bullet):
                        score += 400
                        death = Explosion(EXPLOSION_IMG, [enemy2.pos[0],enemy2.pos[1]],[enemy2.vel[0],enemy2.vel[1]])
                        explosion_list.append(death)
                        if bullet in bullet_list:
                            bullet_list.remove(bullet)
                        if enemy2 in enemy2_list: 
                            enemy2_list.remove(enemy2)
                            ENEMY_DEATH_SND.rewind()
                            ENEMY_DEATH_SND.play()
                        for explosion in explosion_list:
                            death.draw(canvas)
                            death.update()
                            if explosion in explosion_list:    
                                explosion_list.remove(death)
            for enemy3 in enemy3_list:
                    if enemy3.has_collided(bullet):
                        score += 400
                        death = Explosion(EXPLOSION_IMG, [enemy3.pos[0],enemy3.pos[1]],[enemy3.vel[0],enemy3.vel[1]])
                        explosion_list.append(death)
                        if bullet in bullet_list:
                            bullet_list.remove(bullet)
                        if enemy3 in enemy3_list: 
                            enemy3_list.remove(enemy3)
                            ENEMY_DEATH_SND.rewind()
                            ENEMY_DEATH_SND.play()
                        for explosion in explosion_list:
                            death.draw(canvas)
                            death.update()
                            if explosion in explosion_list:    
                                explosion_list.remove(death)
            for boss in boss_list:
                    if boss.has_collided(bullet):
                        score += 200
                        if bullet in bullet_list:
                            bullet_list.remove(bullet)
                        if boss in boss_list and boss.pos[1] >= 30 and boss_escape == False: 
                            boss_health += 1
                            BOSS_HIT_SND.rewind()
                            BOSS_HIT_SND.play()
                            boss_damage = True
                        boss.death()
# This line of code draws an explosion on the player
# position when it dies
        for player_death in player_explosion_list:
            player_death.draw(canvas)
            player_death.update()
            
# This set of code is for text that shows after a game
# has been started
        if player_is_dead == False:
# This set of code is for the transition text, when the
# player transitions to the next stage        
            if hanke < 360: 
                if text_hanker < 60:
                    text_hanker += 1
                    canvas.draw_text("Warmup",(340,200),35,"white","monospace")
                    stage = "Warmup"
            if hanke > 360 and hanke < 2520:
                if text_hanker >= 60 and text_hanker < 120:
                    text_hanker += 1
                    canvas.draw_text("Stage 1",(340,200),35,"white","monospace")
                    canvas.draw_text("Faster Enemies",(280,240),35,"white","monospace")
                    stage = "Stage 1"
            if hanke > 2520 and hanke < 4680:
                if text_hanker >= 120 and text_hanker < 180:
                    text_hanker += 1
                    canvas.draw_text("Stage 2",(340,200),35,"white","monospace")
                    canvas.draw_text("New Enemy",(320,240),35,"white","monospace")
                    stage = "Stage 2"
            if hanke > 4680 and hanke < 6840:
                if text_hanker >= 180 and text_hanker < 240:
                    text_hanker += 1
                    canvas.draw_text("Stage 3",(340,200),35,"white","monospace")
                    canvas.draw_text("New Enemy",(320,240),35,"white","monospace")
                    stage = "Stage 3"
            if hanke %3420 == 0 and hanke >= 6840:
                boss_fight = True
            if boss_fight == True:  
                boss_hanker += 1
                if boss_hanker > 360 and boss_hanker < 540:
                    if boss_hanker % 4 < 2:
                        canvas.draw_text("ALERT",(357,170),35,"red","monospace")
                        canvas.draw_text("Boss Fight",(312,200),35,"red","monospace")
                    if boss_hanker % 4 >= 2:
                        canvas.draw_text("ALERT",(357,170),35,"white","monospace")    
                        canvas.draw_text("Boss Fight",(312,200),35,"white","monospace")
                if boss_fight == 6840:
                    stage = "Boss Fight"
            if hanke > 6840 and boss_fight == False:
                if text_hanker >=240  and text_hanker < 300:
                    text_hanker += 1
                    canvas.draw_text("Endless Mode",(280,200),35,"white","monospace")
                    stage = "Endless Mode"
                    
        if player_is_dead:        
            canvas.draw_text("Press space to restart",(190,600),35,"white","monospace")        
        canvas.draw_text("Score: " + ((str)(score)),(5,780),12,"white","monospace")
        canvas.draw_text("High Score: " + ((str)(highscore)),(5,795),12,"white","monospace")
        canvas.draw_text((stage),(5,765),12,"white","monospace")
    else:
        BOSS.rewind()
        MENU.play()
        GAME.rewind()       
# This set of code is for text that shows after a game
# has been started
        canvas.draw_text("High Score:",(295,200),35,"white","monospace")
        canvas.draw_text(((str)(highscore)),(295,250),35,"white","monospace")
        canvas.draw_text("Press space to start",(205,600),35,"white","monospace")
        canvas.draw_text("Space or J to shoot",(5,30),12,"white","monospace")
        canvas.draw_text("Arrow Keys or WASD to move",(5,15),12,"white","monospace")
        canvas.draw_text("+ FX volume with P",(5,750),12,"white","monospace")
        canvas.draw_text("- FX volume with O",(5,765),12,"white","monospace")
        canvas.draw_text("+ Music volume with I",(5,780),12,"white","monospace")
        canvas.draw_text("- Music volume with U",(5,795),12,"white","monospace")
        canvas.draw_text("To enable invincibility press L",(590,775),12,"white","monospace")
        canvas.draw_image(TITLE_IMG,
                     [TITLE_WIDTH/2,TITLE_HEIGHT/2],
                     [TITLE_WIDTH,TITLE_HEIGHT],
                     [400,100],
                     TITLE_SIZE)
        
    if god_mode:
        canvas.draw_text("Invincibility Enabled",(660,795),12,"white","monospace")
        
new_game()
animated = True
# This creates a frame and assign callbacks to event
# handlers
frame = simplegui.create_frame("Star-Mania", WIDTH, HEIGHT)
frame.set_draw_handler(draw)
frame.set_canvas_background("white")
frame.set_keydown_handler(key_down)
frame.set_keyup_handler(key_up)
# This starts the frame animation
frame.start()
