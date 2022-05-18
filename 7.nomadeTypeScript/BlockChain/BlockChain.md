# Chain

- 이제 체인을 만들어보자!

```jsx
//import * as crypto from "crypto";
import crypto from "crypto";

interface BlockShape {
  hash: string;
  prevHash: string;
  height: number;
  data: string;
}

class Block implements BlockShape {
  public hash: string;
  constructor(
    public prevHash: string,
    public height: number,
    public data: string
  ) {
    this.hash = Block.calculateHash(prevHash, height, data);
  }
  static calculateHash(prevHash: string, height: number, data: string) {
    const toHash = `${prevHash}${height}${data}`;
    return crypto.createHash("sha256").update(toHash).digest("hex");
  }
}

class Blockchain {
  private blocks: Block[];
  constructor() {
    this.blocks = [];
  }
  private getPrevHash() {
    if (this.blocks.length === 0) return "";
    return this.blocks[this.blocks.length - 1].hash;
  }
  public addBlock(data: string) {
    const newBlock = new Block(
      this.getPrevHash(),
      this.blocks.length + 1,
      data
    );
    this.blocks.push(newBlock);
  }
  public getBlocks() {
    return [...this.blocks];
  }
}

const blockchain = new Blockchain();

blockchain.addBlock("First one");
blockchain.addBlock("Second one");
blockchain.addBlock("Third one");
blockchain.addBlock("Fourth one");
blockchain.getBlocks().push(new Block("xxxxxx", 111111, "HACKEDDD")); (여기)

console.log(blockchain.getBlocks());
```

1. 블록체인 클래스를 만든다.
2. Blockchain에는 blocks라는 Private 속성이 하나 있다. blocks는 Block 클래스의 배열이다.
3. 클래스 생성자 파라미터는 비워둔다.
4. `this.blocks = [];` 로 빈 배열을 지정해준다.
5. addBlock 이라는 public 함수 생성한다. ( 새로운 블록을 추가할 땐, 블록에 저장하고 싶은 데이터를 보내줘야 한다.
   1. 이게 새로운 블록을 생성해줄 것이다.
   2. `const newblock = new Block()` 으로 새로운 블록을 생성한다.
   3. 새로운 블록을 생성하려면 이전 해쉬값인 prevHash가 필요하다.
6. 이전 해쉬값을 불러올 수 있는 private 함수 getPrevHash()를 만든다.
   1. blocks의 길이인 `this.blocks.length` 가 0 이면 빈 문자열을 리턴한다.
   2. 그 외엔 `this.blocks[this.blocks.length - 1].hash;` 를 리턴한다. ( 마지막 블럭의 해쉬값 )
7. addBlock 함수로 돌아와서 해쉬값을 입력해준다. `const block = new Block(this.getPrevHash(), this.blocks.length + 1, data);`
   1. 이러면 블록이 생성된다.
   2. 새로운 블록이 생성되면 그 블록을 즉시 해싱한다 (`calculateHash()` 함수에서)
8. 이제 새로 생성된 블록을 blocks 배열에 넣어준다.
   1. `this.blocks.push(newblock)`
9. 블록에 접근 할 수 있는 함수 `getBlocks()` 를 생성한다.
   1. `this.blocks` 를 리턴하면 (여기) 부분처럼 다른 사람이 임의로 블록체인을 생성할 수 있다. 누구든지 여러 단계를 거치지 않고 바로 새로운 블록추가가능

      1. 블록을 리턴할때에 `this.blocks` 를 리턴하는데 이건 블록체인 안의 블록 정보이며 private 값이다.
      2. 하지만 `this.blocks` 를 리턴하면 하게 되면 배열에 접근이 가능하다.
      3. `new Block()` 으로 새로운 블록을 배열에 더하는게 가능하다.

      ```jsx
      Block {
          prevHash: 'xxxxxx',
          height: 111111,
          data: 'HACKEDDDDD',
          hash: 'c4a78e320a4b28b653f66fce909a005c3382f9fae42ba5a09c19c9f36a53febd'
        }
      ```

   2. 그러므로 `[...this.blocks]` 를 리턴하여 완전히 새로운 배열을 리턴하도록 해준다. ( 블록은 다 들어있지만 새로운 배열이다 )
   3. (여기) 에서 배열안에 새로운 블록을 더하고 있지만 `class Blockcghain` 의 state 와 연결되지는 않는다.

<img width="617" alt="image" src="https://user-images.githubusercontent.com/82592845/169017960-0db77ef4-f854-4162-9c1f-74a9756c8caa.png">
