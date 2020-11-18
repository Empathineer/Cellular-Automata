# Cellular-Automata


<p>Implement the full class <strong>Automaton</strong> from the lectures.&nbsp; Test the class in a <strong>main()</strong> that works like the <strong>main()</strong> from the module.&nbsp; Show at least four different sample runs of width 79, giving 100 generations per run (to keep the output manageable). <span style="color: #cc0066;">Show me the runs for rules 4, 126 and 130.&nbsp; Add at least one more rule of your choice.</span></p>
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
<p>The meaning and behavior of members and methods is in the modules, but here is a summary.</p>
<ul>
<li>
<strong>Automaton(int rule)</strong> - a constructor. Through the mutator, below, we'll sanitize <strong>rule</strong> and then convert it to our internal representation. We also need to establish the seed: a single 1 in a sea of 0s (in this assignment a 1 means an asterisk, '*', and a 0 means a blank, ' '. This is accomplished more directly by setting <strong>thisGen</strong> to<span style="font-family: Courier New;"> "*"</span> and <strong>extremeBit</strong> to <span style="font-family: Courier New;">" "</span>.</li>
<li>
<strong>String toStringCurrentGen()</strong> - This returns a string consisting of the current generation, <strong>thisGen</strong>, but does so by embedding it in a return String whose length is <em>exactly</em> <strong>displayWidth</strong> long. If <strong>thisGen</strong> is <em>smaller</em> than <strong>displayWidth</strong>, it is positioned in the center of the larger return string, with the excess padded by <strong>extremeBit</strong> characters. If <strong>thisGen</strong> is <em>longer</em> than <strong>displayWidth</strong>, it has to be truncated (on both ends) so that it is perfectly centered in the returned string, any excess trimmed off both ends, equally. You can easily do this with simple String objects and concatenation.</li>
<li>
<strong>Two Mutators</strong>:
<ul>
<li>
<strong>boolean setRule(int newRule)</strong> - converts the int <strong>newRule</strong>, a number from 0-255, to an array of eight <strong>booleans</strong>.&nbsp; For example, if the <strong>newRule</strong> is <strong>182</strong>, then this is the binary number <strong>1 </strong> <span style="color: #ff0000;"><strong> <span style="background-color: #33cccc;">0</span></strong></span><strong> 1 1 0 1 </strong> <span style="color: #0000ff;"><strong> <span style="background-color: #ffff00;">1</span></strong></span><strong> 0</strong>, which means that the resultant array will be
<blockquote>rule[7] = true, <br> <span style="color: #ff0000;"> <span style="background-color: #33cccc; font-weight: bold;"> rule[6] = false </span> </span>, <br> rule[5] = true, <br> rule[4] = true, <br> rule[3] = false, <br> rule[2] = true, <br> <span style="color: #0000ff;"> <span style="background-color: #ffff00; font-weight: bold;"> rule[1] = true </span> </span> , <br> rule[0] = false.</blockquote>
I have color coded two of the bits in <strong>182</strong> and their corresponding <strong>booleans</strong> in the array <strong>rule[]</strong> so you can easily see how the conversion needs to work.&nbsp; As usual, this must sanitize the <strong>int</strong> so that only values in the legal range 0-255 are allowed.</li>
<li>
<strong>boolean setDisplayWidth(int width)</strong> - this is described by the earlier description of <strong>displayWidth</strong> and <strong>MAX_DISPLAY_WIDTH</strong>. I repeat that only odd widths are allowed (for centering purposes).</li>
</ul>
</li>
<li>
<strong>void propagateNewGeneration()</strong> - this is the workhorse function.&nbsp; It will use the three private members <strong>thisGen</strong>, <strong>extremeBit</strong> and<strong> rule[]</strong>, and create the next generation from them.&nbsp; This method must first append two <strong>extremeBit</strong>s to each side of <strong>thisGen</strong> in order to provide it with enough bits (or chars) on each end needed by <strong>rule</strong>.&nbsp; This adds four chars to <strong>thisGen</strong>, temporarily. We then apply <strong>rule</strong> in a loop to the new, larger, <strong>thisGen, </strong>which creates a separate (local) <strong>nextGen</strong> String. If you do this correctly, <strong>nextGen</strong> will be two characters smaller than the recently augmented <strong>thisGen</strong> (but still two larger than the original <strong>thisGen</strong> that entered the method). [<em>You must understand this statement, conceptually, before you write your first line of code, or you will be doomed.</em>] Then, you replace the old <strong>thisGen</strong> with our new <strong>nextGen</strong>.<br> <br> In brief, we pad <strong>thisGen</strong> with four <strong>extremeBit</strong>s, then apply rule, and we have a new <strong>nextGen</strong>, two larger than we started out with. We copy that back to <strong>thisGen</strong> to complete the cycle.<br> <br> Finally, we have to apply <strong>rule</strong> to<strong><i> three consecutive </i>extremeBits</strong> to figure out what the new <strong>extremeBit</strong> will be for the next generation (" " or "*"?) .&nbsp; What do I mean by "<i>three consecutive</i>"?&nbsp; We apply <strong> rule</strong> to an int representing a 3-bit pattern inside the old generation.&nbsp; In this case, we are doing this way out where all the bits are the same:&nbsp; <strong>extremeBit</strong>.&nbsp; So each input will be three 0s or three 1s -- an int 0 or an int 7 -- depending on the value of <strong>extremeBit</strong>.&nbsp; The result will enable us determine the new value of <strong>extremeBit</strong>.&nbsp; We must do this before we return, so <strong>extremeBit</strong> is correct for the next call to this method.</li>
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
<h4><span style="color: #0000ff;"><span style="font-weight: 400;">Option B1 - rules[32]</span></span></h4>
<p>Do a version that has a larger rules array:&nbsp; <strong>rules[32]</strong>.&nbsp; The larger array coincides with the use of five, rather than three, bits from the previous generation to produce a bit in the next generation.&nbsp; Thus, each new bit is influenced by a wider segment above it.&nbsp; Also, each bit spreads out more in the generation below it, causing the width of <strong>thisGen</strong> to increase faster.&nbsp; Make adjustments to all code above, but use the same class, members and methods.&nbsp; You should be able to determine which rules in your larger rule-space correspond to rules 30, 126 and 4 of the small rule-space of the lectures. Once you have figured out which new rules correspond to the old rules 4, 126 and 30, test those:&nbsp; they should look exactly the same.&nbsp; Then find some new interesting rules that produce output not seen in the 8-bit rules problem.</p>
<h4><span style="color: #0000ff;"><span style="font-weight: 400;">Option B2 - Graphics Output</span></span></h4>
<p>After testing and preparing a console output for submission, try a GUI version using a <strong>Jframe</strong> and swing/awt objects.&nbsp; All the classes and members as <i>spec-ed</i> above must be used without modification.</p>
<pre>Once your assignment is graded and returned, you can view the instructor solution here:</pre>
<ul>
<li title="Click to insert a link to this quiz"><a href="/courses/3209/quizzes" data-api-endpoint="https://foothillcollege.instructure.com/api/v1/courses/3209/quizzes" data-api-returntype="[Quiz]"> Quiz List</a></li>
</ul>
<p>Your access code will be provided in your graded assignment comments.&nbsp; Find the assignment in the list, click "Take Survey" and you will see the solution.&nbsp; Even though it is called a "Quiz," it is actually just a solution; there is no need to submit anything, just open the quiz and see the solution.</p></div>
