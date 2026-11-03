# Cocos Creator 2.4 — Cheatsheet

Docs chính thức: https://docs.cocos.com/creator/2.4/manual/en/ · API: https://docs.cocos.com/creator/2.4/api/en/

## Component cơ bản
```ts
const { ccclass, property } = cc._decorator;

@ccclass
export default class MyComp extends cc.Component {
    @property(cc.Prefab) itemPrefab: cc.Prefab = null;
    @property(cc.SpriteFrame) frame: cc.SpriteFrame = null;
    @property(cc.Label) label: cc.Label = null;

    onLoad() { }          // node vừa được tạo
    start() { }           // trước frame update đầu tiên
    update(dt: number) { }
    onDestroy() { }
}
```

## Load JSON trong `assets/resources`
```ts
cc.resources.load("LevelData/level-0", cc.JsonAsset, (err, asset: cc.JsonAsset) => {
    if (err) { cc.error(err); return; }
    const data = asset.json;
});
```
Docs: [Asset loading](https://docs.cocos.com/creator/2.4/manual/en/scripting/load-assets.html)

## Tạo / huỷ node
```ts
const node = cc.instantiate(this.itemPrefab); // từ prefab
node.parent = this.node;
node.setPosition(0, 100);

const n = new cc.Node("Name");                // từ code
n.addComponent(cc.Sprite).spriteFrame = this.frame;

node.destroy();                               // huỷ ở cuối frame
cc.isValid(node, true);                       // false nếu đã gọi destroy()
```

## Hẹn giờ
```ts
this.scheduleOnce(() => { /* ... */ }, 0.5);  // sau 0.5s
this.unscheduleAllCallbacks();
```

## Touch
```ts
node.on(cc.Node.EventType.TOUCH_START, (e: cc.Event.EventTouch) => { }, this);
node.on(cc.Node.EventType.TOUCH_MOVE, ...);
node.on(cc.Node.EventType.TOUCH_END, ...);
node.on(cc.Node.EventType.TOUCH_CANCEL, ...);

const worldPos = e.getLocation();
const localPos = node.parent.convertToNodeSpaceAR(worldPos);
```
Docs: [Node events](https://docs.cocos.com/creator/2.4/manual/en/scripting/internal-events.html)

## Physics (Box2D)
```ts
const physics = cc.director.getPhysicsManager();
physics.enabled = true;
physics.gravity = cc.v2(0, -960);
physics.debugDrawFlags = cc.PhysicsManager.DrawBits.e_shapeBit; // vẽ collider để debug

const body = node.getComponent(cc.RigidBody);
body.type = cc.RigidBodyType.Dynamic;   // Static | Kinematic | Dynamic
body.linearVelocity = cc.v2(0, 0);

const box = node.addComponent(cc.PhysicsBoxCollider);
box.size = cc.size(200, 80);
box.apply();                            // bắt buộc sau khi đổi thông số bằng code
```
- Group & Collision Matrix: `Project → Project Settings → Group Manager`; set `node.group` trong Inspector.
- Docs: [Physics](https://docs.cocos.com/creator/2.4/manual/en/physics/physics/) · [RigidBody](https://docs.cocos.com/creator/2.4/manual/en/physics/physics/rigid-body.html) · [Collider](https://docs.cocos.com/creator/2.4/manual/en/physics/physics/collider-component.html)

## Debug
- `cc.log(...)`, `cc.warn(...)`, `cc.error(...)` — xem trong Console của Editor hoặc DevTools khi Preview trên browser.
