<script lang="ts">
    // 减少颜色数量，以便更容易区分
    const colorWords = "红,黄,绿,蓝,紫,黑,白,橙".split(",");
    // 预定义的颜色，以便更容易区分
    const colors = (
        "red,green,blue,purple,orange,pink,gray,brown,olive,cyan," +
        "navy,lime,aqua,fuchsia,teal,coral,crimson,gold,indigo,violet," +
        "black"
    ).split(",");

    function getRand(): [string, string] {
        let word = colorWords[Math.floor(Math.random() * colorWords.length)];
        let color = colors[Math.floor(Math.random() * colors.length)];
        return [word, color];
    }

    const maxCount = 10;
    const minCount = 2;
    // 默认生成5个文字
    let inputCount = 5;

    let count;
    const data: [string, string][] = [];

    function genData() {
        // 限制count范围
        count = Math.max(minCount, Math.min(maxCount, inputCount));
        // console.log(count);

        data.length = 0;
        for (let i = 0; i < count; i++) {
            data.push(getRand());
        }
    }

    setTimeout(genData, 200);
</script>

<div class="container">
    <h1>快速读出以下颜色👇</h1>
    <p class="hint">🌈点击刷新，快来试试吧！</p>
    <input
        type="number"
        min={minCount}
        max={maxCount}
        bind:value={inputCount}
    />
    <button onclick={genData}>刷新</button>
    <div class="output">
        {#each data as [word, color]}
            <span style={`color:${color}`}> {word} </span>
        {/each}
    </div>
</div>

<style lang="less">
    div.container {
        font-family: Arial, sans-serif;
        text-align: center;
        margin-top: 50px;

        p.hint {
            font-size: smaller;
            color: #666;
        }

        div.output {
            margin-top: 20px;
            font-size: 4em;
            font-weight: bold;
            // 增加字之间的间隔
            word-spacing: 10px;
            letter-spacing: 10px;
        }
    }
</style>
