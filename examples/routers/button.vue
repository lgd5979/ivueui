<template>
    <div>
        <h4>基本</h4>
        <br><br>
        <Button-group>
            <Button>取消</Button>
            <Button type="primary">确定</Button>
        </Button-group>
        <Button-group>
            <Button disabled>昨日</Button>
            <Button disabled>今日</Button>
            <Button disabled>明日</Button>
        </Button-group>
        <Button-group>
            <Button type="primary">L</Button>
            <Button>M</Button>
            <Button type="ghost">M</Button>
            <Button type="dashed">R</Button>
        </Button-group>
        <br><br>
        <h4>配合图标</h4>
        <br><br>
        <Button-group>
            <Button type="primary">
                <Icon type="chevron-left"></Icon>
                前进
            </Button>
            <Button type="primary">
                后退
                <Icon type="chevron-right"></Icon>
            </Button>
        </Button-group>
        <Button-group>
            <Button type="primary" icon="ios-skipbackward"></Button>
            <Button type="primary" icon="ios-skipforward"></Button>
        </Button-group>
        <Button-group>
            <Button type="ghost" icon="ios-color-wand-outline"></Button>
            <Button type="ghost" icon="ios-sunny-outline"></Button>
            <Button type="ghost" icon="ios-crop"></Button>
            <Button type="ghost" icon="ios-color-filter-outline"></Button>
        </Button-group>
        <br><br>
        <h4>配合图标</h4>
        <br><br>
            <Button type="primary" loading>加载中...</Button>
            <Button type="primary" loading disabled>加载中...</Button>
            <Button type="primary" shape="circle" loading disabled></Button>
            <Button type="primary" shape="circle" loading></Button>
        <br><br>
        <h4>ICon图标</h4>
        <br><br>
        <Button-group>
            <Button type="primary">
                <Icon type="caomei-bug"></Icon>
                BUG
            </Button>
            <Button type="primary">
                USB
                <Icon type="caomei-usb"></Icon>
            </Button>
        </Button-group>
        <br><br>
        <h4>圆角</h4>
        <br><br>
            <Button type="primary" shape="circle">
                <Icon type="chevron-left"></Icon>
            </Button>
        <Button-group shape="circle">
            <Button type="primary">
                <Icon type="chevron-left"></Icon>
                前进
            </Button>
            <Button type="primary">
                后退
                <Icon type="chevron-right"></Icon>
            </Button>
        </Button-group>
        <Button-group shape="circle">
            <Button type="primary" icon="ios-skipbackward"></Button>
            <Button type="primary" icon="ios-skipforward"></Button>
        </Button-group>
        <Button-group shape="circle">
            <Button type="ghost" icon="ios-color-wand-outline"></Button>
            <Button type="ghost" icon="ios-sunny-outline"></Button>
            <Button type="ghost" icon="ios-crop"></Button>
            <Button type="ghost" icon="ios-color-filter-outline"></Button>
        </Button-group>
        <br><br>
        <h4>尺寸</h4>
        <br><br>
        <Button-group size="large">
            <Button type="ghost">Large</Button>
            <Button type="ghost">Large</Button>
        </Button-group>
        <Button-group>
            <Button type="ghost">Default</Button>
            <Button type="ghost">Default</Button>
        </Button-group>
        <Button-group size="small">
            <Button type="ghost">Small</Button>
            <Button type="ghost">Small</Button>
        </Button-group>
        <br><br>
        <Button-group size="large" shape="circle">
            <Button type="ghost">Large</Button>
            <Button type="ghost">Large</Button>
        </Button-group>
        <Button-group shape="circle">
            <Button type="ghost">Default</Button>
            <Button type="ghost">Default</Button>
        </Button-group>
        <Button-group size="small" shape="circle">
            <Button type="ghost">Small</Button>
            <Button type="ghost">Small</Button>
        </Button-group>
    </div>
</template>
<script>
    export default {

    }
</script>
