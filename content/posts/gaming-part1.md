---
title: 🎮 Explorando o Desenvolvimento de Jogos em Go
date: 2024-08-01 # Data de publicação
description: Uma visão geral das principais bibliotecas e frameworks Go para criação de jogos.
tags:
  - Go
  - Jogos
  - GameDev
  - ECS
  - 2D
  - 3D
---


# 🗺️ Back to Content Menu: Explorando o Desenvolvimento de Jogos em Go

Go, com sua performance, concorrência e sintaxe simples, tem se tornado uma escolha interessante para desenvolvimento de jogos. Embora não seja tão dominante quanto C++ ou C#, a comunidade Go tem criado bibliotecas e frameworks poderosos para facilitar a criação de jogos 2D e 3D. Este post apresenta algumas das ferramentas mais notáveis.

## ECS (Entity Component System)

O padrão ECS é uma arquitetura popular no desenvolvimento de jogos que prioriza a composição em vez da herança. Ele organiza os objetos do jogo (entidades) como coleções de componentes de dados, com sistemas que operam nesses componentes.

* **Ark:** Ark é um ECS baseado em arquétipos para Go. Arquétipos são layouts de memória para entidades que compartilham os mesmos componentes, o que permite iteração e processamento de dados muito eficientes.

    ```go
    package main

    import (
        "fmt"
        "[github.com/vknabel/ark](https://github.com/vknabel/ark)"
    )

    // Defina componentes como structs simples.
    type Position struct {
        X, Y float32
    }

    type Velocity struct {
        X, Y float32
    }

    func main() {
        // Crie um mundo Ark.
        world := ark.NewWorld()

        // Registre os tipos de componente.
        positionType := world.RegisterComponent(Position{})
        velocityType := world.RegisterComponent(Velocity{})

        // Crie uma entidade com componentes de posição e velocidade.
        entity := world.NewEntity(positionType, velocityType)

        // Obtenha ponteiros para os componentes da entidade.
        position := (*Position)(world.GetComponent(entity, positionType))
        velocity := (*Velocity)(world.GetComponent(entity, velocityType))

        // Defina valores para os componentes.
        position.X = 10
        position.Y = 20
        velocity.X = 5
        velocity.Y = 0

        // Defina um sistema para atualizar a posição com base na velocidade.
        movementSystem := func(deltaTime float32) {
            query := world.Query(positionType, velocityType)
            for query.Next() {
                entity := query.Entity()
                position := (*Position)(world.GetComponent(entity, positionType))
                velocity := (*Velocity)(world.GetComponent(entity, velocityType))

                position.X += velocity.X * deltaTime
                position.Y += velocity.Y * deltaTime
            }
        }

        // Simule a atualização do jogo.
        deltaTime := 0.1
        movementSystem(float32(deltaTime))

        // Imprima a nova posição.
        fmt.Printf("New Position: X: %f, Y: %f\\n", position.X, position.Y) // Saída: New Position: X: 10.500000, Y: 20.000000
    }
    ```

## Engines 2D

Go oferece opções para desenvolvedores interessados em criar jogos 2D, desde engines simples até bibliotecas mais completas.

* **Ebitengine:** Ebitengine é um engine 2D simples e fácil de usar. Ele fornece funcionalidades básicas como desenho de imagens, manipulação de entrada e reprodução de áudio. É uma ótima opção para iniciantes e projetos menores.

    ```go
    package main

    import (
        "image/color"
        "log"
        "[github.com/hajimehoshi/ebiten/v2](https://github.com/hajimehoshi/ebiten/v2)"
        "[github.com/hajimehoshi/ebiten/v2/ebitenutil](https://github.com/hajimehoshi/ebiten/v2/ebitenutil)"
    )

    const (
        screenWidth  = 640
        screenHeight = 480
    )

    type Game struct{}

    func (g *Game) Update() error {
        // Lógica de atualização do jogo (ex: movimentação, física).
        return nil
    }

    func (g *Game) Draw(screen *ebiten.Image) {
        // Desenha na tela.
        screen.Fill(color.RGBA{0x00, 0x40, 0x80, 0xff}) // Fundo azul escuro.
        ebitenutil.DrawRect(screen, 50, 50, 100, 100, color.White) // Retângulo branco.
        ebitenutil.DrawCircle(screen, 320, 240, 50, color.RGBA{0xff, 0x00, 0x00, 0xff}) // Círculo vermelho.
        ebitenutil.DrawText(screen, "Hello, Ebitengine!", 100, 200, nil, color.White) // Texto
    }

    func (g *Game) Layout(outsideWidth, outsideHeight int) (int, int) {
        return screenWidth, screenHeight
    }

    func main() {
        // Cria uma nova janela do jogo.
        ebiten.SetWindowSize(screenWidth, screenHeight)
        ebiten.SetWindowTitle("Simple Game with Ebitengine")

        // Executa o loop do jogo.
        if err := ebiten.RunGame(&Game{}); err != nil {
            log.Fatal(err)
        }
    }
    ```

* **engo:** Engo é um engine 2D mais completo que segue o paradigma ECS. Ele oferece recursos como gerenciamento de cenas, sistemas de animação e suporte a física básica.

    ```go
    package main

    import (
        "log"
        "engo.io/engo"
        "engo.io/engo/common"
        "image/color"
    )

    type MyScene struct{}

    func (s *MyScene) Preload() {
        // Carrega assets do jogo.
    }

    func (s *MyScene) Setup(u engo.Updater) error {
        w, _ := engo.GetSize()
        world := &u.World()

        // Adiciona o sistema de renderização.
        common.SetBackground(color.White)
        world.AddSystem(&common.RenderSystem{})

        // Cria uma entidade com um componente de posição e um componente de renderização.
        square := engo.NewEntity()
        square.AddComponent(&common.SpaceComponent{
            Position: engo.Point{X: w / 2, Y: w / 2},
            Width:    100,
            Height:   100,
        })
        square.AddComponent(&common.RenderComponent{
            Color: color.RGBA{0xff, 0x00, 0x00, 0xff},
            Priority: 0,
        })

        // Adiciona a entidade ao mundo.
        world.AddEntity(square)

        return nil
    }

    func (s *MyScene) Type() string { return "MyScene" }

    func main() {
        opts := engo.RunOptions{
            Title:  "Engo Example",
            Width:  600,
            Height: 600,
        }
        if err := engo.Run(opts, &MyScene{}); err != nil {
            log.Fatal(err)
        }
    }
    ```

## Engines 3D

Para jogos com gráficos 3D, Go também possui algumas opções, embora o cenário não seja tão vasto quanto o de engines 2D.

* **g3n:** G3N é um engine 3D para Go. Ele oferece suporte a renderização, materiais, iluminação e animação.

    ```go
    package main

    import (
        "log"
        "[github.com/g3n/engine/app](https://github.com/g3n/engine/app)"
        "[github.com/g3n/engine/camera](https://github.com/g3n/engine/camera)"
        "[github.com/g3n/engine/core](https://github.com/g3n/engine/core)"
        "[github.com/g3n/engine/geometry](https://github.com/g3n/engine/geometry)"
        "[github.com/g3n/engine/gls](https://github.com/g3n/engine/gls)"
        "[github.com/g3n/engine/light](https://github.com/g3n/engine/light)"
        "[github.com/g3n/engine/material](https://github.com/g3n/engine/material)"
        "[github.com/g3n/engine/math32](https://github.com/g3n/engine/math32)"
        "[github.com/g3n/engine/renderer](https://github.com/g3n/engine/renderer)"
        "[github.com/g3n/engine/scene](https://github.com/g3n/engine/scene)"
        "[github.com/g3n/engine/util/helper](https://github.com/g3n/engine/util/helper)"
        "[github.com/g3n/engine/window](https://github.com/g3n/engine/window)"
    )

    func main() {
        // Cria uma nova aplicação G3N.
        a := app.App()
        defer a.Dispose()

        // Cria uma nova janela.
        w, h := a.GetSize()
        win := a.GetWindow()
        win.SetTitle("G3N Example")

        // Cria um novo sistema de renderização.
        gs, err := gls.New("opengl")
        if err != nil {
            log.Fatal(err)
        }
        defer gs.Dispose()

        // Cria uma nova cena.
        scn := scene.New(gs)

        // Cria uma nova câmera.
        cam := camera.New(gs, w, h)
        cam.SetPosition(0, 0, 5)
        scn.Add(cam)

        // Cria uma luz ambiente.
        ambientLight := light.NewAmbient(gs, color.RGBA{255, 255, 255, 255}, 0.5)
        scn.Add(ambientLight)

        // Cria uma geometria de cubo.
        geom := geometry.NewCube(gs, 1, 1, 1)

        // Cria um material para o cubo.
        mat := material.NewStandard(gs)
        mat.SetColor(color.RGBA{0x00, 0xff, 0x00, 0xff}) // Verde

        // Cria um mesh (objeto) do cubo.
        cube := core.NewMesh(gs, geom, mat)
        scn.Add(cube)

        // Adiciona um helper para visualizar os eixos.
        helper := helper.New(gs, 3)
        scn.Add(helper)

        // Define a função de loop de renderização.
        a.Run(func(renderer *renderer.Renderer, deltaTime float32) {
            gs.Clear(color.RGBA{0x44, 0x44, 0x44, 0xff}) // Cinza escuro
            renderer.Render(scn, cam)
            cube.RotateY(deltaTime) // Rotaciona o cubo
        })
    }
    ```

* **Harfang3D:** Harfang3D é um engine 3D para a linguagem Go, com suporte para Windows e Linux. Harfang3D é um engine mais completo, adequado para projetos maiores e mais ambiciosos. Ele oferece um conjunto robusto de recursos, incluindo renderização avançada, sistema de física e ferramentas de edição.

    ```go
    package main

    import (
        "log"
        "runtime"

        "[github.com/harfang3d/harfang-go/v3/hcore](https://github.com/harfang3d/harfang-go/v3/hcore)"
        "[github.com/harfang3d/harfang-go/v3/hgui](https://github.com/harfang3d/harfang-go/v3/hgui)"
        "[github.com/harfang3d/harfang-go/v3/hrender](https://github.com/harfang3d/harfang-go/v3/hrender)"
    )

    func main() {
        // Inicializa o Harfang.
        hcore.Init()
        defer hcore.Release()

        // Cria uma janela.
        win, err := hgui.NewWindow(1920, 1080, "Harfang Go - Cube", hgui.WindowDefault)
        if err != nil {
            log.Fatal(err)
        }
        defer win.Close()

        // Inicializa o renderizador.
        renderer, err := hrender.NewRenderer(hcore.Get(), hrender.Config{
            Debug:            true,
            EnableVR:         false,
            EnableValidation: true,
        })
        if err != nil {
            log.Fatal(err)
        }
        defer renderer.Release()

        // Cria uma cena.
        scene := hrender.NewScene()

        // Cria uma câmera.
        cameraNode := hcore.NewNode()
        cameraNode.SetName("camera")
        cameraNode.SetTransform(math3d.Translation(0, 0, -5))
        scene.GetRoot().AddChild(cameraNode)
        camera := hrender.NewCamera(renderer, hrender.CameraPerspective, 45, 0.1, 1000)
        camera.SetNode(cameraNode)
        scene.SetCurrentCamera(camera)

        // Cria um cubo.
        cubeNode := hcore.NewNode()
        cubeNode.SetName("cube")
        cubeNode.SetTransform(math3d.Identity())
        scene.GetRoot().AddChild(cubeNode)
        cubeGeometry := hrender.CreateCubeGeometry(renderer, 1, 1, 1)
        cubeMaterial := hrender.NewMaterial(renderer, hrender.DefaultMaterial)
        cubeMaterial.SetColor("u_base_color", hcore.Color{R: 255, G: 0, B: 0, A: 255}) // Vermelho
        cube := hrender.NewMesh(renderer, cubeNode, cubeGeometry, cubeMaterial)
        scene.AddModel(cube)

        // Cria uma luz direcional.
        lightNode := hcore.NewNode()
        lightNode.SetName("light")
        lightNode.SetTransform(math3d.RotationX(math32.Pi / 4))
        scene.GetRoot().AddChild(lightNode)
        directionalLight := hrender.NewLight(renderer, hrender.LightDirectional)
        directionalLight.SetColor(hcore.Color{R: 255, G: 255, B: 255, A: 255})
        directionalLight.SetIntensity(1)
        directionalLight.SetNode(lightNode)
        scene.AddLight(directionalLight)

        // Loop principal do jogo.
        for {
            // Atualiza o Harfang.
            hcore.Update()

            // Verifica se a janela foi fechada.
            if hgui.IsAppQuit() || win.WasClosed() {
                break
            }

            // Obtém o tamanho da janela.
            width, height := win.GetSize()

            // Atualiza a câmera.
            camera.SetView(0, 0, width, height)
            camera.Update()

            // Atualiza a cena.
            scene.Update(float32(hcore.GetDeltaTime()))
            cubeNode.SetTransform(math3d.Mul(cubeNode.GetTransform(), math3d.RotationY(float32(hcore.GetDeltaTime()))))

            // Renderiza a cena.
            renderer.Draw(scene)

            // Exibe o frame.
            win.Flip()

            // Limpa o renderizador.
            renderer.Clear(hcore.Color{R: 0, G: 0, B: 0, A: 255}, hrender.ClearDepth)
        }
    }
    ```

## Recursos Adicionais

Além dos engines, Go oferece bibliotecas e ferramentas úteis para tarefas específicas no desenvolvimento de jogos.

* **go-sdl2:** Go-SDL2 são bindings Go para a biblioteca SDL2, uma biblioteca multiplataforma para acesso de baixo nível a hardware de áudio, vídeo e entrada.

    ```go
    package main

    import (
        "log"
        "[github.com/veandco/go-sdl2/sdl](https://github.com/veandco/go-sdl2/sdl)"
    )

    func main() {
        // Inicializa o SDL2.
        if err := sdl.Init(sdl.INIT_EVERYTHING); err != nil {
            log.Fatal(err)
        }
        defer sdl.Quit()

        // Cria uma janela.
        window, err := sdl.CreateWindow("SDL2 Example", sdl.WINDOWPOS_UNDEFINED, sdl.WINDOWPOS_UNDEFINED,
            800, 600, sdl.WINDOW_SHOWN)
        if err != nil {
            log.Fatal(err)
        }
        defer window.Destroy()

        // Cria um renderizador.
        renderer, err := sdl.CreateRenderer(window, -1, sdl.RENDERER_ACCELERATED)
        if err != nil {
            log.Fatal(err)
        }
        defer renderer.Destroy()

        // Define a cor de desenho para vermelho.
        renderer.SetDrawColor(255, 0, 0, 255)

        // Limpa a tela com a cor de desenho.
        renderer.Clear()

        // Desenha um retângulo vermelho.
        rect := sdl.Rect{X: 100, Y: 100, W: 200, H: 100}
        renderer.FillRect(&rect)

        // Exibe o conteúdo do renderizador na janela.
        renderer.Present()

        // Aguarda por um evento de saída.
        running := true
        for running {
            for event := sdl.PollEvent(); event != nil; event = sdl.PollEvent() {
                switch event.(type) {
                case *sdl.QuitEvent:
                    running = false
                    break
                }
            }
        }
    }
    ```

* **Pixel:** Pixel é uma biblioteca 2D feita à mão em Go. Ele fornece uma API simples e eficiente para desenho de gráficos 2D.

    ```go
    package main

    import (
        "[github.com/faiface/pixel](https://github.com/faiface/pixel)"
        "[github.com/faiface/pixel/pixelgl](https://github.com/faiface/pixel/pixelgl)"
        "[github.com/faiface/pixel/imdraw](https://github.com/faiface/pixel/imdraw)"
        "golang.org/x/image/colornames"
        "log"
    )

    func run() {
        // Cria uma nova janela do Pixel.
        cfg := pixelgl.WindowConfig{
            Title:  "Pixel Example",
            Bounds: pixel.R(0, 0, 800, 600),
            VSync:  true,
        }
        win, err := pixelgl.NewWindow(&cfg)
        if err != nil {
            log.Fatal(err)
        }

        // Cria um novo ImDraw (desenho imediato).
        imd := imdraw.New(nil)

        // Loop principal do jogo.
        for !win.Closed() {
            // Limpa a janela.
            win.Clear(colornames.Black)

            // Desenha um triângulo.
            imd.Clear()
            imd.Color = colornames.Red
            imd.Push(pixel.V(100, 100), pixel.V(200, 300), pixel.V(300, 100))
            imd.Polygon(0) // 0 significa não preenchido, use um valor maior para preencher

            // Desenha um círculo preenchido.
            imd.Color = colornames.Blue
            imd.Push(pixel.V(400, 300))
            imd.Circle(50, 0) // Raio 50, 0 significa preenchido

            // Desenha o ImDraw na janela.
            imd.Draw(win)

            // Atualiza a janela.
            win.Update()
        }
    }

    func main() {
        // Executa a função run na thread principal.
        pixelgl.Run(run)
    }
    ```

* **raylib-go:** Raylib-go são bindings Go para raylib, uma biblioteca simples e fácil de usar para aprender programação de videogames.

    ```go
    package main

    import (
        "[github.com/gen2brain/raylib-go/raylib](https://github.com/gen2brain/raylib-go/raylib)"
        "runtime"
    )

    func main() {
        // Inicializa o raylib.
        raylib.InitWindow(800, 600, "Raylib Example")
        defer raylib.CloseWindow()

        // Define a cor de fundo.
        raylib.SetTargetFPS(60)

        // Loop principal do jogo.
        for !raylib.WindowShouldClose() {
            // Inicia o desenho.
            raylib.BeginDrawing()

            // Limpa a tela.
            raylib.ClearBackground(raylib.RayWhite)

            // Desenha um círculo.
            raylib.DrawCircle(400, 300, 100, raylib.Red)

            // Desenha um retângulo.
            raylib.DrawRectangle(100, 100, 200, 100, raylib.Blue)

            // Desenha um texto.
            raylib.DrawText("Hello, Raylib!", 200, 400, 20, raylib.Green)

            // Termina o desenho.
            raylib.EndDrawing()
        }
    }
    ```

## Utilidades

* **fantasyname:** Fantasyname é um gerador de nomes de fantasia.

    ```go
    package main

    import (
        "fmt"
        "[github.com/NebulousLabs/fantasyname](https://github.com/NebulousLabs/fantasyname)"
    )

    func main() {
        // Gera um nome de fantasia aleatório.
        name := fantasyname.GenerateFantasyName()
        fmt.Println("Nome de fantasia:", name)

        // Gera um nome de fantasia com uma semente específica.
        nameWithSeed := fantasyname.GenerateFantasyNameSeed(12345)
        fmt.Println("Nome de fantasia com semente 12345:", nameWithSeed)
    }
    ```

* **go-astar:** Go A\* é uma implementação em Go do algoritmo de busca de caminho A\*, útil para IA e movimentação de personagens.

    ```go
    package main

    import (
        "fmt"
        "[github.com/beefsack/go-astar](https://github.com/beefsack/go-astar)"
        "math"
    )

    // Defina o mundo do problema como uma grade.
    type Grid struct {
        Rows, Cols int
        Walls      map[int]bool // Mapa de células de parede, indexado por row*numCols+col
    }

    // Defina um nó na grade.
    type Node struct {
        Row, Col int
    }

    // Função auxiliar para calcular o índice da célula.
    func (g Grid) index(r, c int) int {
        return r*g.Cols + c
    }

    // Implemente a interface astar.Pather para Node.

    // PathNeighbors retorna os nós vizinhos válidos.
    func (n Node) PathNeighbors(p astar.Pather) []astar.Pather {
        g := p.(Grid)
        neighbors := []Node{
            {n.Row - 1, n.Col},
            {n.Row + 1, n.Col},
            {n.Row, n.Col - 1},
            {n.Row, n.Col + 1},
        }
        validNeighbors := []astar.Pather{}
        for _, neighbor := range neighbors {
            if neighbor.Row >= 0 && neighbor.Row < g.Rows && neighbor.Col >= 0 && neighbor.Col < g.Cols && !g.Walls[g.index(neighbor.Row, neighbor.Col)] {
                validNeighbors = append(validNeighbors, neighbor)
            }
        }
        return validNeighbors
    }

    // PathCost retorna o custo de mover para um vizinho.
    func (n Node) PathCost(p astar.Pather, to astar.Pather) float64 {
        return 1
    }

    // PathEstimatedCostTo retorna uma estimativa heurística do custo para o destino.
    func (n Node) PathEstimatedCostTo(p astar.Pather, to astar.Pather) float64 {
        t := to.(Node)
        return math.Abs(float64(n.Row-t.Row)) + math.Abs(float64(n.Col-t.Col)) // Distância de Manhattan
    }

    func main() {
        // Defina uma grade de exemplo com paredes.
        grid := Grid{
            Rows: 5,
            Cols: 5,
            Walls: map[int]bool{
                6:  true,
                7:  true,
                8:  true,
                11: true,
                16: true,
                17: true,
                18: true,
            },
        }

        // Defina os nós inicial e de destino.
        start := Node{0, 0}
        end := Node{4, 4}

        // Encontre o caminho usando A*.
        path, distance, found := astar.Path(start, end, grid)
        if !found {
            fmt.Println("Caminho não encontrado")
            return
        }

        // Imprima o caminho.
        fmt.Println("Caminho encontrado com distância:", distance)
        for _, p := range path {node := p.(Node)
            fmt.Printf("(%d,%d) ", node.Row, node.Col)
        }
        fmt.Println()
    }
    ```

* **go3d:** Go3d é um pacote Go otimizado para matemática 2D/3D.

    ```go
    package main

    import (
        "fmt"
        "[github.com/ungerik/go3d/f32](https://github.com/ungerik/go3d/f32)"
    )

    func main() {
        // Cria vetores 3D.
        v1 := f32.Vec3{1, 2, 3}
        v2 := f32.Vec3{4, 5, 6}

        // Imprime os vetores.
        fmt.Println("Vetor 1:", v1)




# 🗺️ Back to Content Menu: Explorando o Desenvolvimento de Jogos em Go

Go, com sua performance, concorrência e sintaxe simples, tem se tornado uma escolha interessante para desenvolvimento de jogos. Embora não seja tão dominante quanto C++ ou C#, a comunidade Go tem criado bibliotecas e frameworks poderosos para facilitar a criação de jogos 2D e 3D. Este post apresenta algumas das ferramentas mais notáveis.

## ECS (Entity Component System)

O padrão ECS é uma arquitetura popular no desenvolvimento de jogos que prioriza a composição em vez da herança. Ele organiza os objetos do jogo (entidades) como coleções de componentes de dados, com sistemas que operam nesses componentes.

* **Ark:** [Ark](https://github.com/vknabel/ark) é um ECS baseado em arquétipos para Go. Arquétipos são layouts de memória para entidades que compartilham os mesmos componentes, o que permite iteração e processamento de dados muito eficientes.

## Engines 2D

Go oferece opções para desenvolvedores interessados em criar jogos 2D, desde engines simples até bibliotecas mais completas.

* **Ebitengine:** [Ebitengine](https://ebiten.org/) é um engine 2D simples e fácil de usar. Ele fornece funcionalidades básicas como desenho de imagens, manipulação de entrada e reprodução de áudio. É uma ótima opção para iniciantes e projetos menores.
* **engo:** [Engo](https://github.com/EngoEngine/engo) é um engine 2D mais completo que segue o paradigma ECS. Ele oferece recursos como gerenciamento de cenas, sistemas de animação e suporte a física básica.

## Engines 3D

Para jogos com gráficos 3D, Go também possui algumas opções, embora o cenário não seja tão vasto quanto o de engines 2D.

* **g3n:** [G3N](https://github.com/g3n/engine) é um engine 3D para Go. Ele oferece suporte a renderização, materiais, iluminação e animação.

* **Harfang3D:** [Harfang3D](https://www.harfang3d.com/doc/v3.2/go.html) é um engine 3D para a linguagem Go, com suporte para Windows e Linux.

## Recursos Adicionais

Além dos engines, Go oferece bibliotecas e ferramentas úteis para tarefas específicas no desenvolvimento de jogos.

* **go-sdl2:** [Go-SDL2](https://github.com/veandco/go-sdl2) são bindings Go para a biblioteca SDL2, uma biblioteca multiplataforma para acesso de baixo nível a hardware de áudio, vídeo e entrada.
* **Pixel:** [Pixel](https://github.com/faiface/pixel) é uma biblioteca 2D feita à mão em Go. Ele fornece uma API simples e eficiente para desenho de gráficos 2D.
* **raylib-go:** [Raylib-go](https://github.com/gen2brain/raylib-go) são bindings Go para raylib, uma biblioteca simples e fácil de usar para aprender programação de videogames.

## Utilidades

* **fantasyname:** [Fantasyname](https://github.com/NebulousLabs/fantasyname) é um gerador de nomes de fantasia.
* **go-astar:** [Go A\*](https://github.com/beefsack/go-astar) é uma implementação em Go do algoritmo de busca de caminho A\*, útil para IA e movimentação de personagens.
* **go3d:** [Go3d](https://github.com/ungerik/go3d) é um pacote Go otimizado para matemática 2D/3D.
* **grid:** [Grid](https://github.com/MidnightDead/grid) é uma biblioteca genérica de grade 2D com ray-casting, shadow-casting e pathfinding.
* **tile:** [Tile](https://github.com/kelvinmwinuka/tile) é uma biblioteca de grade 2D orientada a dados e amigável ao cache (TileMap), que inclui pathfinding, observadores e importação/exportação.
* **prototype:** [Prototype](https://github.com/gonutz/prototype) é uma biblioteca multiplataforma (Windows/Linux/Mac) para criar jogos de desktop usando uma API mínima.
* **termloop:** [Termloop](https://github.com/JoelOtter/termloop) é um engine de jogo baseado em terminal para Go, construído sobre o Termbox.

## Frameworks de Servidor

Para jogos online, Go também oferece frameworks para construir a infraestrutura do servidor.

* **gonet:** [Gonet](https://github.com/xtaci/gonet) é um esqueleto de servidor de jogo implementado com Go.
* **goworld:** [Goworld](https://github.com/xiaoxiaoyaoawm/goworld) é um engine de servidor de jogo escalável, com framework de espaço-entidade e hot-swapping.
* **Leaf:** [Leaf](https://github.com/name5566/leaf) é um framework de servidor de jogo leve.
* **nano:** [Nano](https://github.com/lonng/nano) é um framework de servidor de jogo leve, de alta performance e fácil de usar, baseado em Go.
* **Pitaya:** [Pitaya](https://github.com/topfreegames/pitaya) é um framework de servidor de jogo escalável com suporte a clusterização e bibliotecas de cliente para iOS, Android, Unity e outros através do SDK C.

## Conclusão

Embora o ecossistema de jogos em Go ainda esteja em desenvolvimento, ele oferece um conjunto diversificado de ferramentas para desenvolvedores. Se você está interessado em performance, concorrência e uma sintaxe simples, vale a pena explorar o desenvolvimento de jogos com Go.



