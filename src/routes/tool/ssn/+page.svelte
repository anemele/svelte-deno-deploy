<script lang="ts">
    const COEFFICIENT = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
    const LAST_NUMBER = "10X987654321";

    function calcCheckCode(id17: string): string {
        let sum = 0;
        for (let i = 0; i < 17; i++) {
            sum += parseInt(id17[i]) * COEFFICIENT[i];
        }
        return LAST_NUMBER[sum % 11];
    }

    function checkId18(id18: string): boolean {
        return calcCheckCode(id18.substring(0, 17)) === id18[17].toUpperCase();
    }

    const AREA_CODE =
        "11北京市,12天津市,13河北省,14山西省,15内蒙古,21辽宁省,22吉林省,23黑龙江,31上海市,32江苏省,33浙江省,34安徽省,35福建省,36江西省,37山东省,41河南省,42湖北省,43湖南省,44广东省,45广西,46海南省,50重庆市,51四川省,52贵州省,53云南省,54西藏,61陕西省,62甘肃省,63青海省,64宁夏,65新疆,71台湾省,71台北市,81香港,82澳门";
    const areaCode: Map<string, string> = new Map();
    AREA_CODE.split(",").forEach((item) => {
        areaCode.set(item.substring(0, 2), item.substring(2));
    });

    function normalizeId(id: string): string | null {
        if (
            id.length === 18 &&
            /^\d{17}[0-9Xx]$/.test(id) &&
            areaCode.has(id.substring(0, 2)) &&
            checkId18(id)
        )
            return id.toUpperCase();
        if (
            id.length === 17 &&
            /^\d{17}$/.test(id) &&
            areaCode.has(id.substring(0, 2))
        )
            return id + calcCheckCode(id);
        return null;
    }

    interface IdInfo {
        id: string;
        area: string;
        birth: string;
        sex: string;
    }

    function handleInput(idStr: string) {
        let id = normalizeId(idStr);
        if (!id) {
            idInfo = null;
            return;
        }

        // Area has been checked above
        let area = areaCode.get(id.substring(0, 2))!;

        // Check birth date
        let year = parseInt(id.substring(6, 10));
        let month = parseInt(id.substring(10, 12));
        let day = parseInt(id.substring(12, 14));
        if (
            year < 1900 ||
            year > new Date().getFullYear() ||
            month > 12 ||
            day > 31
        ) {
            idInfo = null;
            return;
        }
        let birth = `${year}年${month}月${day}日`;

        // Get sex
        let sex = parseInt(id.substring(16, 17)) % 2 === 0 ? "女" : "男";

        // Update id info
        idInfo = {
            id: id,
            area: area,
            birth: birth,
            sex: sex,
        };
    }

    let idStr = "";
    let idInfo: IdInfo | null = null;

    $: if (idStr) {
        handleInput(idStr);
    }
</script>

<div class="container">
    <h1>身份证号工具</h1>
    <ul>
        <li class="hint">输入二代身份证号，解析信息</li>
        <li class="hint">输入二代身份证号前17位，计算校验码并解析信息</li>
    </ul>
    <input type="text" bind:value={idStr} />
    <div class="output">
        {#if idInfo}
            <table>
                <tbody>
                    <tr>
                        <td class="key">身份证号</td>
                        <td class="val">{idInfo.id}</td>
                    </tr>
                    <tr>
                        <td class="key">地区</td>
                        <td class="val">{idInfo.area}</td>
                    </tr>
                    <tr>
                        <td class="key">出生日期</td>
                        <td class="val">{idInfo.birth}</td>
                    </tr>
                    <tr>
                        <td class="key">性别</td>
                        <td class="val">{idInfo.sex}</td>
                    </tr>
                </tbody>
            </table>
        {:else if idStr}
            输入不合法
        {/if}
    </div>
</div>

<style lang="less">
    div.container {
        font-family: Arial, sans-serif;
        text-align: center;
        margin-top: 50px;

        ul {
            margin-left: auto;
            margin-right: auto;
            max-width: fit-content;

            text-align: left;

            li.hint {
                font-size: smaller;
                color: #666;
            }
        }

        input {
            margin-top: 30px;
            border: 2px solid #ccc;
        }

        div.output {
            margin-top: 50px;
            margin-left: auto;
            margin-right: auto;
            max-width: fit-content;
            letter-spacing: 2px;

            table {
                td.key {
                    text-align: right;
                    padding-right: 10px;
                }
                td.val {
                    text-align: left;
                    padding-left: 10px;
                }
            }
        }
    }
</style>
