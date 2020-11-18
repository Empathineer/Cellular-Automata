# Cellular-Automata

<h3><u>Objective</u></h3>

<p>The purpose here is to implement the full class <strong>Automaton</strong> from the lectures.&nbsp; In order to test the class in, at least four different sample runs of width 79, giving 100 generations per run needed to be generated. <span style="color: #cc0066;">Per requirementes, rules 4, 126 and 130 needed to be displayed.&nbsp; The last one was of our choice.</span></p>
<p>Here is a repeat of the specification of the class.</p>
<div style="border-style: solid; border-width: 1px; padding: 1px 4px 1px 4px;">
<pre>class Automaton
{
   // class constants
   public final static int MAX_DISPLAY_WIDTH = 121;
   
   // private members
   private boolean rules[];  // allocate rules[8] in constructor!
   private String thisGen;   // same here
   String extremeBit; // bit, "*" or " ", implied everywhere "outside"
   int displayWidth;  // an odd number so it can be perfectly centered
   
   // public constructors, mutators, etc. (need to be written)
   public Automaton(int new_rule)
   public void resetFirstGen()
   public boolean setRule(int new_rule)
   public boolean setDisplayWidth(int width)
   public String toStringCurrentGen()
   public void propagateNewGeneration()
}
</pre>
</div>
<p>Below summarizes the behavior of the members and methods within the Automaton class.</p>
<ul>
<li>
<strong>Automaton(int rule)</strong> - a constructor. The mutator sanitizes the <strong>rule</strong> and then converts it to our internal representation. The seed is then established: a single 1 in a sea of 0s (in this assignment a 1 means an asterisk, '*', and a 0 means a blank, ' '. This is accomplished more directly by setting <strong>thisGen</strong> to<span style="font-family: Courier New;"> "*"</span> and <strong>extremeBit</strong> to <span style="font-family: Courier New;">" "</span>.</li>
<li>
<strong>String toStringCurrentGen()</strong> - This returns a string consisting of the current generation, <strong>thisGen</strong>, but does so by embedding it in a return String whose length is <em>exactly</em> <strong>displayWidth</strong> long. If <strong>thisGen</strong> is <em>smaller</em> than <strong>displayWidth</strong>, it is positioned in the center of the larger return string, with the excess padded by <strong>extremeBit</strong> characters. If <strong>thisGen</strong> is <em>longer</em> than <strong>displayWidth</strong>, it has to be truncated (on both ends) so that it is perfectly centered in the returned string, any excess trimmed off both ends, equally. </li>
<li>
<strong>Two Mutators</strong>:
<ul>
<li>
<strong>boolean setRule(int newRule)</strong> - converts the int <strong>newRule</strong>, a number from 0-255, to an array of eight <strong>booleans</strong>.&nbsp; For example, if the <strong>newRule</strong> is <strong>182</strong>, then this is the binary number <strong>1 </strong> <span style="color: #ff0000;"><strong> <span style="background-color: #33cccc;">0</span></strong></span><strong> 1 1 0 1 </strong> <strong> <span style="color:blue";">1</span></strong></span><strong> 0</strong>, which means that the resultant array will be
<blockquote>rule[7] = true, <br> <span style="color: #ff0000;"> <span style="background-color: #33cccc; font-weight: bold;"> rule[6] = false </span> </span>, <br> rule[5] = true, <br> rule[4] = true, <br> rule[3] = false, <br> rule[2] = true, <br> <span style="color: #0000ff;"> <span style="background-color: #ffff00; font-weight: bold;"> rule[1] = true </span> </span> , <br> rule[0] = false.</blockquote>
</li>
<li>
<strong>boolean setDisplayWidth(int width)</strong> - this is described by the earlier description of <strong>displayWidth</strong> and <strong>MAX_DISPLAY_WIDTH</strong>. I repeat that only odd widths are allowed (for centering purposes).</li>
</ul>
</li>
<li>
<strong>void propagateNewGeneration()</strong> - this is the workhorse function.&nbsp; It will use the three private members <strong>thisGen</strong>, <strong>extremeBit</strong> and<strong> rule[]</strong>, and create the next generation from them.&nbsp; This method first appended two <strong>extremeBit</strong>s to each side of <strong>thisGen</strong> in order to provide it with enough bits (or chars) on each end needed by <strong>rule</strong>.&nbsp; Four chars were added to <strong>thisGen</strong>, temporarily. The <strong>rule</strong> was then applied in a loop to the new, larger, <strong>thisGen, </strong>which created a separate (local) <strong>nextGen</strong> String. As a result, <strong>nextGen</strong> will be two characters smaller than the augmented <strong>thisGen</strong> (but still two larger than the original <strong>thisGen</strong> that entered the method). [<em>You must understand this statement, conceptually, before you write your first line of code, or you will be doomed.</em>] Then, you replace the old <strong>thisGen</strong> with our new <strong>nextGen</strong>.<br> <br> In brief, we pad <strong>thisGen</strong> with four <strong>extremeBit</strong>s, then apply rule, and we have a new <strong>nextGen</strong>, two larger than we started out with. We copy that back to <strong>thisGen</strong> to complete the cycle.<br> <br> Finally, we have to apply <strong>rule</strong> to<strong><i> three consecutive </i>extremeBits</strong> to figure out what the new <strong>extremeBit</strong> will be for the next generation (" " or "*"?) .&nbsp; What do I mean by "<i>three consecutive</i>"?&nbsp; We apply <strong> rule</strong> to an int representing a 3-bit pattern inside the old generation.&nbsp; In this case, we are doing this way out where all the bits are the same:&nbsp; <strong>extremeBit</strong>.&nbsp; So each input will be three 0s or three 1s -- an int 0 or an int 7 -- depending on the value of <strong>extremeBit</strong>.&nbsp; The result will enable us determine the new value of <strong>extremeBit</strong>.&nbsp; We must do this before we return, so <strong>extremeBit</strong> is correct for the next call to this method.</li>
</ul>
<p>Supply a sample set of at least four runs which include the&nbsp; rules, 4, 126, 130, and one or more of your choice.&nbsp; One run would look something like this:</p>
<div style="border-style: solid; border-width: 1px; padding: 1px 4px 1px 4px;">
<pre>Enter Rule (0 - 255): 124
   start
                                     *
                                     **
                                     ***
                                     * **
                                     *****
                                     *   **
                                     **  ***
                                     *** * **
                                     * *******
                                     ***     **
                                     * **    ***
                                     *****   * **
                                     *   **  *****
                                     **  *** *   **
                                     *** * ****  ***
                                     * *****  ** * **
                                     ***   ** ********
                                     * **  ****      **
                                     ***** *  **     ***
                                     *   **** ***    * **
                                     **  *  *** **   *****
                                     *** ** * *****  *   **
                                     * ********   ** **  ***
                                     ***      **  ****** * **
                                     * **     *** *    *******
                                     *****    * ****   *     **
                                     *   **   ***  **  **    ***
                                     **  ***  * ** *** ***   * **
                                     *** * ** ****** *** **  *****
                                     * ********    *** ***** *   **
                                     ***      **   * ***   ****  ***
                                     * **     ***  *** **  *  ** * **
                                     *****    * ** * ***** ** ********
                                     *   **   ********   ******      **
                                     **  ***  *      **  *    **     ***
                                     *** * ** **     *** **   ***    * **
                                     * **********    * *****  * **   *****

   end</pre>
</div>
<p>The above pattern is shorter than an actual run that 100 generations would produce.</p>
<p>Most of you will be able to solve the problem using nothing more than the information contained in the Modules.&nbsp;&nbsp; Spend time trying to do that first.&nbsp; If you get stuck, there are a couple hints you can access.&nbsp; Read them and study them, but only if you need them:</p>
<ul>
<li><a href="http://www.fgamedia.org/faculty/loceff/cs_courses/common/LIFE/15b_Assignment_3_HINT_A.htm" target="_blank" class="external" rel="noreferrer noopener"><span> Understanding How To Apply A Rule to a String</span><span aria-hidden="true" class="ui-icon ui-icon-extlink ui-icon-inline" title="Links to an external site."></span><span class="screenreader-only">&nbsp;(Links to an external site.)</span></a></li>
<li><a href="http://www.fgamedia.org/faculty/loceff/cs_courses/common/LIFE/15b_Assignment_3_HINT_B.htm" target="_blank" class="external" rel="noreferrer noopener"><span> Understanding How To Implement setRule()</span><span aria-hidden="true" class="ui-icon ui-icon-extlink ui-icon-inline" title="Links to an external site."></span><span class="screenreader-only">&nbsp;(Links to an external site.)</span></a></li>
</ul>
</div>
