import turtle

def draw_triangle(t, x, y, size, color=None):

   if color:
       t.fillcolor(color)
       t.begin_fill()

   t.penup()
   t.goto(x, y)
   t.pendown()

   for _ in range(3):
       t.forward(size)
       t.left(120)


   if color:
       t.end_fill()


def sierpinski(t, x, y, size, depth, change_depth):
   if depth == 0:
       draw_triangle(t, x, y, size)
   else:
       half = size / 2
       height = (size / 2) * (3**0.5)


       sierpinski(t, x, y, half, depth - 1, change_depth)
       sierpinski(t, x + half, y, half, depth - 1, change_depth)
       sierpinski(t, x + half / 2, y + height / 2, half, depth - 1, change_depth)


       if depth == change_depth:
           draw_triangle(t, x, y, half, 'red')
           draw_triangle(t, x + half, y, half, 'blue')
           draw_triangle(t, x + half / 2, y + height / 2, half, 'magenta')



t = turtle.Turtle()
t.speed(0)


depth = 2
sierpinski(t, -200, -200, 400, depth, depth)


turtle.done()