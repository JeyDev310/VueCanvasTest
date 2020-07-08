<template>
    <div class="canvasWrapper">
        <canvas id="myCanvas" width="800px" height="480px" style="border:1px solid #c3c3c3;"></canvas>
    </div>
</template>
<script>




/*
 * Instructions: complete all TODOs.
 * 
 * Feel free to upgrade the code if you see areas of opportunity.
 * (Don't assume the starter code is good.)
 * 
 * This VueJS component draws boxes on an HTML canvas
 * See boxes example image in folder
 * A box is defined by it's min and max x/y points.
 * 
 * Bonus points - mock up input data and construct a test case.
 *
*/

import Vue from 'vue'
export default

Vue.extend({
    props: ["ord", "box_list", "current_box", "refresh", "mouse_position",
    "draw_mode", "canvas_transform", "show_annotations"],

    /* ord, order of drawing on canvas
    * box_list, list of dictionaries,
    * current_box, dictionary,
    * refresh, integer / hack for forcing reactive property change
    * mouse_position, dictionary, x, y
    * draw_mode, boolean
    * canvas_transform, dictionary,
    * show_annotations, boolean
    * */

    created() {
    },
    mounted() {
        this.init();
   },
    data() {
        return {
            canvasCtx: null,
            backImage: null
        }
    },
    methods: {
        init: function() {
            var myCanvas = document.getElementById("myCanvas");
            this.canvasCtx = myCanvas.getContext("2d");
            this.backImage = new Image();
            this.backImage.src = 'assets/images/catbackground.png';
            var self = this;
            this.backImage.onload = () => {
            }
            setTimeout(this.drawAll.bind(this), 100);
        },
        drawAll: function() {
            var myCanvas = document.getElementById("myCanvas");
            this.canvasCtx.drawImage(this.backImage, 0, 0, myCanvas.offsetWidth, myCanvas.offsetHeight);
            this.draw(this.canvasCtx, null);
        },
        draw: function (ctx, done) {
            if (this.show_annotations == true) {

                // TODO complete function, will convert n to integer
                var toInt =function(n) {return Math.round(n)};

                let circle_size = 8 / this.canvas_transform['scale']
                let font_size = 20 / this.canvas_transform['scale']
                ctx.font = font_size + "px Verdana";

                var draw_circle = function (circle_size, x, y, ctx) {
                    // TODO complete this function
                    ctx.beginPath();
                    ctx.arc(x, y, circle_size, 0, 2 * Math.PI, true);
                    ctx.fill();
                    ctx.stroke();
                }

                let boxes = this.box_list; // TODO grab box_list

                for (var i in boxes) {

                    let box = boxes[i]

                    if (box.soft_delete != true) {
                        if (box.label.is_visible == null || box.label.is_visible == true) {

                            ctx.beginPath()
                            ctx.lineWidth = '2'

                            let r = box.label.colour.rgba.r
                            let g = box.label.colour.rgba.g
                            let b = box.label.colour.rgba.b
                            // TODO get other colours
                        

                            if (box.selected == true) {
                                ctx.fillStyle = "rgba(0, 0, 200, 1)";
                            } else {
                                ctx.fillStyle = "rgba(" + r + "," + g + "," + b + ", 1)";
                            }

                            if (box.selected == true) {
                                ctx.strokeStyle = "blue"
                            } else {
                                ctx.strokeStyle = box.label.colour.hex
                            }

                            ctx.textAlign = "start";

                            // TODO handle if label is undefined
                            ctx.fillText(box.label.name, toInt(box.x_min), toInt(box.y_min));

                            // TODO draw circles (using eariler created function) at box.[x/y]_min and box.[x/y]_max
                            draw_circle(circle_size, box.x_min, box.y_min, ctx);
                            draw_circle(circle_size, box.x_max, box.y_max, ctx);
                            ////

                            if (box.selected == true) {
                                ctx.fillStyle = "rgba(0, 0, 200, 0.2)";
                            } else {
                                ctx.fillStyle = "rgba(" + r + "," + g + "," + b + ", 0.2)";
                            }
                            // TODO draw dashed line if special condition is true else draw solid line

                            if (box.special_condition == true) {
                                ctx.setLineDash([5, 3]);
                            }
                             else {
                                ctx.setLineDash([0, 0]);                                 
                            }

                            ctx.fill()

                            // TODO draw rectangle
                            ctx.rect(box.x_min, box.y_min, box.x_max-box.x_min, box.y_max-box.y_min);
                            ctx.fill()

                            ctx.closePath()

                            ctx.stroke()
                        }
                    }

                }
            }
            //done();
        }
    }

})
</script>

<style scoped>
.canvasWrapper {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
}
</style>
