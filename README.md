[合集 \- 前端使用 Konva 实现可视化设计器(22\)](https://github.com)[1\.前端使用 Konva 实现可视化设计器（2）\- 参考线、svg、gif图片加载04\-06](https://github.com/xachary/p/18117864)[2\.前端使用 Konva 实现可视化设计器（1）\- 无限画布、比例尺04\-05](https://github.com/xachary/p/18115479)[3\.前端使用 Konva 实现可视化设计器（4）\- 快捷键移动元素04\-11](https://github.com/xachary/p/18130149)[4\.前端使用 Konva 实现可视化设计器（3）\- 单选、多选、选择框04\-10](https://github.com/xachary/p/18127777)[5\.前端使用 Konva 实现可视化设计器（5）\- 磁贴效果04\-16](https://github.com/xachary/p/18139413)[6\.前端使用 Konva 实现可视化设计器（6）\- 复制粘贴、删除、位置、zIndex调整04\-20](https://github.com/xachary/p/18147185)[7\.前端使用 Konva 实现可视化设计器（7）\- 导入导出、上一步、下一步04\-24](https://github.com/xachary/p/18156521)[8\.前端使用 Konva 实现可视化设计器（8）\- 预览框04\-30](https://github.com/xachary/p/18168732)[9\.前端使用 Konva 实现可视化设计器（9）\- 另存为SVG05\-07](https://github.com/xachary/p/18178115)[10\.前端使用 Konva 实现可视化设计器（10）\- 对齐线05\-11](https://github.com/xachary/p/18187292)[11\.前端使用 Konva 实现可视化设计器（11）\- 对齐效果05\-18](https://github.com/xachary/p/18199767)[12\.前端使用 Konva 实现可视化设计器（12）\- 连接线 \- 直线06\-02](https://github.com/xachary/p/18226877)[13\.前端使用 Konva 实现可视化设计器（13）\- 折线 \- 最优路径应用【思路篇】06\-08](https://github.com/xachary/p/18238704)[14\.前端使用 Konva 实现可视化设计器（14）\- 折线 \- 最优路径应用【代码篇】06\-11](https://github.com/xachary/p/18241664):[FlowerCloud机场](https://hushicha.org)[15\.前端使用 Konva 实现可视化设计器（15）\- 自定义连接点、连接优化06\-14](https://github.com/xachary/p/18248511)[16\.前端使用 Konva 实现可视化设计器（16）\- 旋转对齐、触摸板操作的优化06\-17](https://github.com/xachary/p/18253370)[17\.前端使用 Konva 实现可视化设计器（17）\- 素材嵌套 \- 生成阶段07\-19](https://github.com/xachary/p/18312538)[18\.前端使用 Konva 实现可视化设计器（18）\- 素材嵌套 \- 加载阶段07\-22](https://github.com/xachary/p/18316657)[19\.前端使用 Konva 实现可视化设计器（19）\- 连接线 \- 直线、折线08\-01](https://github.com/xachary/p/18337764)[20\.前端使用 Konva 实现可视化设计器（20）\- 性能优化、UI 美化08\-08](https://github.com/xachary/p/18348696)[21\.前端使用 Konva 实现可视化设计器（21）\- 绘制图形（椭圆）08\-20](https://github.com/xachary/p/18370023)22\.前端使用 Konva 实现可视化设计器（22）\- 绘制图形（矩形、直线、折线）09\-10收起
本章分享一下如何使用 Konva 绘制基础图形：矩形、直线、折线，希望大家继续关注和支持哈！



> 请大家动动小手，给我一个免费的 Star 吧\~
> 
> 
> 大家如果发现了 Bug，欢迎来提 Issue 哟\~
> 
> 
> [github源码](https://github.com)
> 
> 
> [gitee源码](https://github.com)
> 
> 
> [示例地址](https://github.com)


## 矩形


先上效果！


![image](https://img2024.cnblogs.com/blog/271236/202409/271236-20240910160316380-1008738887.gif)
![image](https://img2024.cnblogs.com/blog/271236/202409/271236-20240910160323142-1760830900.gif)


实现方式基本和[《前端使用 Konva 实现可视化设计器（21）\- 绘制图形（椭圆）》](https://github.com)是一致的，主要区别矩形的大小和椭圆形的大小设置方式不一样，特别是矩形无需设置 offset。其它就不再赘述了哈。


## 直线、折线


先上效果！


![image](https://img2024.cnblogs.com/blog/271236/202409/271236-20240910160419465-830864575.gif)
![image](https://img2024.cnblogs.com/blog/271236/202409/271236-20240910160423789-371724923.gif)


简单描述一下上面的交互：


首先，绘制一条直线，淡出画一条直线还是比较简单的，根据记录鼠标按下的位置和鼠标释放的位置，就很容易得到 Konva.Line 的 points 应该设定的值了。


然后，沿用绘制 椭圆形、矩形 的思路，它只有特定的 2 个“调整点”，分别代表 起点 和 终点。



```
// src/Render/graphs/Line.ts

// 略

/**
 * 直线、折线
 */
export class Line extends BaseGraph {
  // 略

  constructor(render: Types.Render, dropPoint: Konva.Vector2d) {
    super(render, dropPoint, {
      type: Types.GraphType.Line,
      // 定义了 2 个 调整点
      anchors: [{ adjustType: 'start' }, { adjustType: 'end' }].map((o) => ({
        adjustType: o.adjustType // 调整点 类型定义
      })),
      linkAnchors: [
        { x: 0, y: 0, alias: 'start' },
        { x: 0, y: 0, alias: 'end' }
      ] as Types.AssetInfoPoint[]
    })

    // 新建 直线、折线
    this.line = new Konva.Line({
      name: 'graph',
      x: 0,
      y: 0,
      stroke: 'black',
      strokeWidth: 1,
      hitStrokeWidth: render.toStageValue(5)
    })

    // 给予 1 像素，防止导出图片 toDataURL 失败
    this.group.size({
      width: 1,
      height: 1
    })

    // 加入
    this.group.add(this.line)
    // 鼠标按下位置 作为起点
    this.group.position(this.dropPoint)
  }

  // 实现：拖动进行时
  override drawMove(point: Konva.Vector2d): void {
    // 鼠标拖动偏移量
    const offsetX = point.x - this.dropPoint.x,
      offsetY = point.y - this.dropPoint.y

    // 起点、终点
    const linkPoints = [
      [this.line.x(), this.line.y()],
      [this.line.x() + offsetX, this.line.y() + offsetY]
    ]

    // 直线、折线 路径
    this.line.points(_.flatten(linkPoints))

    // 更新 图形 的 调整点 的 锚点位置
    Line.updateAnchorShadows(this.group, this.anchorShadows, this.line)

    // 更新 图形 的 连接点 的 锚点位置
    Line.updateLinkAnchorShadows(this.group, this.linkAnchorShadows, this.line)

    // 重绘
    this.render.redraw([Draws.GraphDraw.name, Draws.LinkDraw.name, Draws.PreviewDraw.name])
  }

  // 实现：拖动结束
  override drawEnd(): void {
    if (this.line.width() <= 1 && this.line.height() <= 1) {
      // 加入只点击，无拖动

      // 默认大小
      const width = Line.size,
        height = width

      // 起点、终点
      const linkPoints = [
        [this.line.x(), this.line.y()],
        [this.line.x() + width, this.line.y() + height]
      ]

      // 直线、折线 位置大小
      this.line.points(_.flatten(linkPoints))
    }

    // 更新 调整点（拐点）
    Line.updateAnchor(this.render, this.group)

    // 更新 图形 的 调整点 的 锚点位置
    Line.updateAnchorShadows(this.group, this.anchorShadows, this.line)

    // 更新 图形 的 连接点 的 锚点位置
    Line.updateLinkAnchorShadows(this.group, this.linkAnchorShadows, this.line)

    // 对齐线清除
    this.render.attractTool.alignLinesClear()

    // 更新历史
    this.render.updateHistory()

    // 重绘
    this.render.redraw([Draws.GraphDraw.name, Draws.LinkDraw.name, Draws.PreviewDraw.name])
  }

  // 略
}


```

调整点，可以改变 直线、折线 的 起点、终点。



```
// 略

/**
 * 直线、折线
 */
export class Line extends BaseGraph {
  // 实现：更新 图形 的 调整点 的 锚点位置
  static override updateAnchorShadows(
    graph: Konva.Group,
    anchorShadows: Konva.Circle[],
    shape?: Konva.Line
  ): void {
    if (shape) {
      const points = shape.points()
      //
      for (const shadow of anchorShadows) {
        switch (shadow.attrs.adjustType) {
          case 'start':
            shadow.position({
              x: points[0],
              y: points[1]
            })
            break
          case 'end':
            shadow.position({
              x: points[points.length - 2],
              y: points[points.length - 1]
            })
            break
        }
      }
    }
  }
  
  // 略

  // 实现：生成 调整点
  static override createAnchorShapes(
    render: Types.Render,
    graph: Konva.Group,
    anchorAndShadows: {
      anchor: Types.GraphAnchor
      anchorShadow: Konva.Circle
      shape?: Konva.Shape
    }[],
    adjustAnchor?: Types.GraphAnchor
  ): {
    anchorAndShadows: {
      anchor: Types.GraphAnchor
      anchorShadow: Konva.Circle
      shape?: Konva.Shape | undefined
    }[]
  } {
    // stage 状态
    const stageState = render.getStageState()

    const graphShape = graph.findOne('.graph') as Konva.Line

    if (graphShape) {
      const points = graphShape.points()

      for (const anchorAndShadow of anchorAndShadows) {
        let rotate = 0
        const { anchor, anchorShadow } = anchorAndShadow

        const x = render.toStageValue(anchorShadow.getAbsolutePosition().x - stageState.x),
          y = render.toStageValue(anchorShadow.getAbsolutePosition().y - stageState.y)

        if (anchor.adjustType === 'manual') {
          // 略
        } else {
          if (anchor.adjustType === 'start') {
            rotate = Line.calculateAngle(points[2] - points[0], points[3] - points[1])
          } else if (anchor.adjustType === 'end') {
            rotate = Line.calculateAngle(
              points[points.length - 2] - points[points.length - 4],
              points[points.length - 1] - points[points.length - 3]
            )
          }

          const cos = Math.cos((rotate * Math.PI) / 180)
          const sin = Math.sin((rotate * Math.PI) / 180)

          const offset = render.toStageValue(render.pointSize + 5)

          const offsetX = offset * sin
          const offsetY = offset * cos

          const anchorShape = new Konva.Circle({
            name: 'anchor',
            anchor: anchor,
            //
            fill:
              adjustAnchor?.adjustType === anchor.adjustType && adjustAnchor?.groupId === graph.id()
                ? 'rgba(0,0,255,0.8)'
                : 'rgba(0,0,255,0.2)',
            radius: render.toStageValue(3),
            strokeWidth: 0,
            // 位置
            x: x,
            y: y,
            offsetX:
              anchor.adjustType === 'start' ? offsetX : anchor.adjustType === 'end' ? -offsetX : 0,
            offsetY:
              anchor.adjustType === 'start' ? offsetY : anchor.adjustType === 'end' ? -offsetY : 0,
            // 旋转角度
            rotation: graph.getAbsoluteRotation()
          })

          anchorShape.on('mouseenter', () => {
            anchorShape.fill('rgba(0,0,255,0.8)')
            document.body.style.cursor = 'move'
          })
          anchorShape.on('mouseleave', () => {
            anchorShape.fill(
              anchorShape.attrs.adjusting ? 'rgba(0,0,255,0.8)' : 'rgba(0,0,255,0.2)'
            )
            document.body.style.cursor = anchorShape.attrs.adjusting ? 'move' : 'default'
          })

          anchorAndShadow.shape = anchorShape
        }
      }
    }

    return { anchorAndShadows }
  }

  // 略

  // 实现：调整 图形
  static override adjust(
    render: Types.Render,
    graph: Konva.Group,
    graphSnap: Konva.Group,
    adjustShape: Konva.Shape,
    anchorAndShadows: {
      anchor: Types.GraphAnchor
      anchorShadow: Konva.Circle
      shape?: Konva.Shape | undefined
    }[],
    startPoint: Konva.Vector2d,
    endPoint: Konva.Vector2d
  ) {
    // 目标 直线、折线
    const line = graph.findOne('.graph') as Konva.Line
    // 镜像
    const lineSnap = graphSnap.findOne('.graph') as Konva.Line

    // 调整点 锚点
    const anchors = (graph.find('.anchor') ?? []) as Konva.Circle[]
    // 镜像
    const anchorsSnap = (graphSnap.find('.anchor') ?? []) as Konva.Circle[]

    // 连接点 锚点
    const linkAnchors = (graph.find('.link-anchor') ?? []) as Konva.Circle[]

    if (line && lineSnap) {
      // stage 状态
      const stageState = render.getStageState()

      {
        const [graphRotation, adjustType, ex, ey] = [
          Math.round(graph.rotation()),
          adjustShape.attrs.anchor?.adjustType,
          endPoint.x,
          endPoint.y
        ]

        const { x: cx, y: cy, width: cw, height: ch } = graphSnap.getClientRect()

        const { x, y } = graph.position()

        const [centerX, centerY] = [cx + cw / 2, cy + ch / 2]

        const { x: sx, y: sy } = Line.rotatePoint(ex, ey, centerX, centerY, -graphRotation)
        const { x: rx, y: ry } = Line.rotatePoint(x, y, centerX, centerY, -graphRotation)

        const points = line.points()
        const manualPoints = (line.attrs.manualPoints ?? []) as Types.LineManualPoint[]

        if (adjustType === 'manual') {
          // 略
        } else {
          const anchor = anchors.find((o) => o.attrs.adjustType === adjustType)
          const anchorShadow = anchorsSnap.find((o) => o.attrs.adjustType === adjustType)

          if (anchor && anchorShadow) {
            {
              const linkPoints = [
                [points[0], points[1]],
                ...manualPoints.sort((a, b) => a.index - b.index).map((o) => [o.x, o.y]),
                [points[points.length - 2], points[points.length - 1]]
              ]

              switch (adjustType) {
                case 'start':
                  {
                    linkPoints[0] = [sx - rx, sy - ry]
                    line.points(_.flatten(linkPoints))
                  }
                  break
                case 'end':
                  {
                    linkPoints[linkPoints.length - 1] = [sx - rx, sy - ry]
                    line.points(_.flatten(linkPoints))
                  }
                  break
              }
            }
          }
        }
      }

      // 更新 调整点（拐点）
      Line.updateAnchor(render, graph)

      // 更新 调整点 的 锚点 位置
      Line.updateAnchorShadows(graph, anchors, line)

      // 更新 图形 的 连接点 的 锚点位置
      Line.updateLinkAnchorShadows(graph, linkAnchors, line)

      // 更新 调整点 位置
      for (const anchor of anchors) {
        for (const { shape } of anchorAndShadows) {
          if (shape) {
            if (shape.attrs.anchor?.adjustType === anchor.attrs.adjustType) {
              const anchorShadow = graph
                .find(`.anchor`)
                .find((o) => o.attrs.adjustType === anchor.attrs.adjustType)

              if (anchorShadow) {
                shape.position({
                  x: render.toStageValue(anchorShadow.getAbsolutePosition().x - stageState.x),
                  y: render.toStageValue(anchorShadow.getAbsolutePosition().y - stageState.y)
                })
                shape.rotation(graph.getAbsoluteRotation())
              }
            }
          }
        }
      }

      // 重绘
      render.redraw([Draws.GraphDraw.name, Draws.LinkDraw.name, Draws.PreviewDraw.name])
    }
  }

  // 略
}


```

### 折线


相比绘制 椭圆形、矩形 比较不一样的地方在于，椭圆形、矩形 的“调整点”是固定的，而绘制 折线 不一样，没调整一个新的拐点，就会新增 2 个新调整点，整体交互与 手动连接线 类似。


![image](https://img2024.cnblogs.com/blog/271236/202409/271236-20240910160441189-2026784590.gif)



```
// src/Render/draws/GraphDraw.ts

// 略

export interface GraphDrawState {
  // 略

  /**
   * 调整中 调整点
   */
  adjustAnchor?: Types.GraphAnchor

  /**
   * 鼠标按下 调整点 位置
   */
  startPointCurrent: Konva.Vector2d

  /**
   * 图形 group
   */
  graphCurrent?: Konva.Group

  /**
   * 图形 group 镜像，用于计算位置、大小的偏移
   */
  graphCurrentSnap?: Konva.Group
}

// 略

export class GraphDraw extends Types.BaseDraw implements Types.Draw {
  // 略

  state: GraphDrawState = {
    adjusting: false,
    adjustGroupId: '',
    startPointCurrent: { x: 0, y: 0 }
  }

  // 略

  override draw() {
    this.clear()
    // 所有图形
    const graphs = this.render.layer
      .find('.asset')
      .filter((o) => o.attrs.assetType === Types.AssetType.Graph) as Konva.Group[]

    for (const graph of graphs) {
      // 非选中状态才显示 调整点
      if (!graph.attrs.selected) {
        // 略

        for (const anchorAndShadow of anchorAndShadows) {
          const { shape } = anchorAndShadow

          if (shape) {
            // 鼠标按下
            shape.on('mousedown', () => {
              const pos = this.getStagePoint()
              if (pos) {
                this.state.adjusting = true
                this.state.adjustAnchor = shape.attrs.anchor
                this.state.adjustGroupId = graph.id()

                this.state.startPointCurrent = pos

                this.state.graphCurrent = graph
                this.state.graphCurrentSnap = graph.clone()

                shape.setAttr('adjusting', true)

                if (this.state.adjustAnchor) {
                  switch (shape.attrs.anchor?.type) {
                    case Types.GraphType.Line:
                      // 使用 直线、折线 静态处理方法
                      Graphs.Line.adjustStart(this.render, graph, this.state.adjustAnchor, pos)
                      break
                  }
                }
              }
            })

            // 略

            // 调整结束
            this.render.stage.on('mouseup', () => {
              // 略
              
              this.state.adjusting = false
              this.state.adjustAnchor = undefined
              this.state.adjustGroupId = ''

              // 恢复显示所有 调整点
              for (const { shape } of anchorAndShadows) {
                if (shape) {
                  shape.opacity(1)
                  shape.setAttr('adjusting', false)
                  if (shape.attrs.anchor?.type === Types.GraphType.Line) {
                    if (shape.attrs.anchor.adjusted) {
                      shape.fill('rgba(0,0,0,0.4)')
                    } else {
                      shape.fill('rgba(0,0,255,0.2)')
                    }
                  } else {
                    shape.stroke('rgba(0,0,255,0.2)')
                  }
                }

                // 略
              }

              // 略
            })

            // 略
          }
        }
      }
    }
  }
}

```

上面除了需要更多的状态记录 调整 信息，还需要定义 Line 特有的 adjustStart 方法：



```
// src/Render/graphs/Line.ts

// 略

/**
 * 直线、折线
 */
export class Line extends BaseGraph {
  // 略

  /**
   * 调整之前
   */
  static adjustStart(
    render: Types.Render,
    graph: Konva.Group,
    adjustAnchor: Types.GraphAnchor & { manualIndex?: number; adjusted?: boolean },
    endPoint: Konva.Vector2d
  ) {
    const { x: gx, y: gy } = graph.position()

    const shape = graph.findOne('.graph') as Konva.Line

    if (shape && typeof adjustAnchor.manualIndex === 'number') {
      const manualPoints = (shape.attrs.manualPoints ?? []) as Types.LineManualPoint[]
      if (adjustAnchor.adjusted) {
        //
      } else {
        manualPoints.push({
          x: endPoint.x - gx,
          y: endPoint.y - gy,
          index: adjustAnchor.manualIndex
        })
        shape.setAttr('manualPoints', manualPoints)
      }

      // 更新 调整点（拐点）
      Line.updateAnchor(render, graph)
    }
  }
}

// 略

```

动态的调整点，会记录在 line 的 attrs 中 manualPoints，每次首次调整一处 拐点，就会新增一个 新 拐点，主要应用在：



```
// 略

/**
 * 直线、折线
 */
export class Line extends BaseGraph {
  // 略

  // 实现：调整 图形
  static override adjust(
    render: Types.Render,
    graph: Konva.Group,
    graphSnap: Konva.Group,
    adjustShape: Konva.Shape,
    anchorAndShadows: {
      anchor: Types.GraphAnchor
      anchorShadow: Konva.Circle
      shape?: Konva.Shape | undefined
    }[],
    startPoint: Konva.Vector2d,
    endPoint: Konva.Vector2d
  ) {
    // 目标 直线、折线
    const line = graph.findOne('.graph') as Konva.Line
    // 镜像
    const lineSnap = graphSnap.findOne('.graph') as Konva.Line

    // 调整点 锚点
    const anchors = (graph.find('.anchor') ?? []) as Konva.Circle[]
    // 镜像
    const anchorsSnap = (graphSnap.find('.anchor') ?? []) as Konva.Circle[]

    // 连接点 锚点
    const linkAnchors = (graph.find('.link-anchor') ?? []) as Konva.Circle[]

    if (line && lineSnap) {
      // stage 状态
      const stageState = render.getStageState()

      {
        const [graphRotation, adjustType, ex, ey] = [
          Math.round(graph.rotation()),
          adjustShape.attrs.anchor?.adjustType,
          endPoint.x,
          endPoint.y
        ]

        const { x: cx, y: cy, width: cw, height: ch } = graphSnap.getClientRect()

        const { x, y } = graph.position()

        const [centerX, centerY] = [cx + cw / 2, cy + ch / 2]

        const { x: sx, y: sy } = Line.rotatePoint(ex, ey, centerX, centerY, -graphRotation)
        const { x: rx, y: ry } = Line.rotatePoint(x, y, centerX, centerY, -graphRotation)

        const points = line.points()
        const manualPoints = (line.attrs.manualPoints ?? []) as Types.LineManualPoint[]

        if (adjustType === 'manual') {
          if (adjustShape.attrs.anchor?.manualIndex !== void 0) {
            const index = adjustShape.attrs.anchor?.adjusted
              ? adjustShape.attrs.anchor?.manualIndex
              : adjustShape.attrs.anchor?.manualIndex + 1

            const manualPointIndex = manualPoints.findIndex((o) => o.index === index)

            if (manualPointIndex > -1) {
              manualPoints[manualPointIndex].x = sx - rx
              manualPoints[manualPointIndex].y = sy - ry
            }

            const linkPoints = [
              [points[0], points[1]],
              ...manualPoints.sort((a, b) => a.index - b.index).map((o) => [o.x, o.y]),
              [points[points.length - 2], points[points.length - 1]]
            ]

            line.setAttr('manualPoints', manualPoints)

            line.points(_.flatten(linkPoints))

            //
            const adjustAnchorShadow = anchors.find(
              (o) => o.attrs.adjustType === 'manual' && o.attrs.manualIndex === index
            )
            if (adjustAnchorShadow) {
              adjustAnchorShadow.position({
                x: sx - rx,
                y: sy - ry
              })
            }
          }
        } else {
          // 略
        }
      }

      // 略
    }
  }

  // 略

  /**
   * 更新 调整点（拐点）
   * @param render
   * @param graph
   */
  static updateAnchor(render: Types.Render, graph: Konva.Group) {
    const anchors = graph.attrs.anchors ?? []
    const anchorShadows = graph.find('.anchor') ?? []

    const shape = graph.findOne('.graph') as Konva.Line

    if (shape) {
      // 已拐
      let manualPoints = (shape.attrs.manualPoints ?? []) as Types.LineManualPoint[]
      const points = shape.points()

      // 调整点 + 拐点
      const linkPoints = [
        [points[0], points[1]],
        ...manualPoints.sort((a, b) => a.index - b.index).map((o) => [o.x, o.y]),
        [points[points.length - 2], points[points.length - 1]]
      ]

      // 清空 调整点（拐点）,保留 start end
      anchors.splice(2)
      const shadows = anchorShadows.splice(2)
      for (const shadow of shadows) {
        shadow.remove()
        shadow.destroy()
      }

      manualPoints = []

      for (let i = linkPoints.length - 1; i > 0; i--) {
        linkPoints.splice(i, 0, [])
      }

      // 调整点（拐点）
      for (let i = 1; i < linkPoints.length - 1; i++) {
        const anchor = {
          type: graph.attrs.graphType,
          adjustType: 'manual',
          //
          name: 'anchor',
          groupId: graph.id(),
          //
          manualIndex: i,
          adjusted: false
        }

        if (linkPoints[i].length === 0) {
          anchor.adjusted = false

          // 新增
          const prev = linkPoints[i - 1]
          const next = linkPoints[i + 1]

          const circle = new Konva.Circle({
            adjustType: anchor.adjustType,
            anchorType: anchor.type,
            name: anchor.name,
            manualIndex: anchor.manualIndex,
            radius: 0,
            // radius: render.toStageValue(2),
            // fill: 'red',
            //
            x: (prev[0] + next[0]) / 2,
            y: (prev[1] + next[1]) / 2,
            anchor
          })

          graph.add(circle)
        } else {
          anchor.adjusted = true

          // 已拐
          const circle = new Konva.Circle({
            adjustType: anchor.adjustType,
            anchorType: anchor.type,
            name: anchor.name,
            manualIndex: anchor.manualIndex,
            adjusted: true,
            radius: 0,
            // radius: render.toStageValue(2),
            // fill: 'red',
            //
            x: linkPoints[i][0],
            y: linkPoints[i][1],
            anchor
          })

          graph.add(circle)

          manualPoints.push({
            x: linkPoints[i][0],
            y: linkPoints[i][1],
            index: anchor.manualIndex
          })
        }

        anchors.push(anchor)
      }

      shape.setAttr('manualPoints', manualPoints)

      graph.setAttr('anchors', anchors)
    }
  }

  // 略
}


```

上面简单的说，就是处理 manualPoints 的算法，负责控制新增拐点，然后把“点”们插入到 起点、终点 之间，最后处理成 Konva.Line 的 points 的值。


顺带一说。区分 起点、终点 和 拐点 是通过 attrs 中的 adjustType 字段；区分 拐点 是否已经操作过 是通过 attrs 中的 adjusted 字段；拐点是存在明确的顺序的，会记录在 attrs 的 manualIndex 字段中。



> 个人觉得，目前，绘制图形的 代码结构 和 变量命名 容易产生歧义，后面尽量抽出时间重构一下，大家支持支持 👇！


Thanks watching\~



> More Stars please！勾勾手指\~
> 
> 
> [源码](https://github.com)
> 
> 
> [gitee源码](https://github.com)
> 
> 
> [示例地址](https://github.com)


