---
title: BioJava:CookbookFrench:Interfaces:Coordinates
---

Comment afficher les coordonnées d'une Sequence?
------------------------------------------------

Lorsqu'il faut afficher une séquence, il est utile d'afficher les
coordonnées d'une séquence pour pouvoir vous permettre de naviguer au
travers de cette séquence. BioJava contient une implémentation de
*SequenceRenderer* appelée *RulerRenderer* qui affiche les coordonnées
d'une *Sequence*.

Parce qu'un *SequenceRenderContext* ne peut utiliser qu'un seul
*SequenceRenderer* à la fois, vous devrez utilisé un
*MultiLineRenderer*. Un *MultiLineRenderer* implémente
*SequenceRenderer* et peut encapsuler plusieurs *SequenceRenderers* en
coordonnant leur affichage en plusieurs pistes.

L'usage d'un *RulerRenderer* et d'un *MultiLineRenderer* est montré dans
le programme ci-dessous. Un exemple d'affichage de l'interface graphique
se trouve sous le programme.

<java> import java.awt.\*; import java.awt.event.\*; import
javax.swing.\*;

import org.biojava.bio.gui.sequence.\*; import org.biojava.bio.seq.\*;
import org.biojava.bio.symbol.\*;

public class MultiView extends JFrame {

` private JPanel jPanel = new JPanel();`  
` private MultiLineRenderer mlr = new MultiLineRenderer();`  
` private SequenceRenderer symR = new SymbolSequenceRenderer();`  
` private RulerRenderer ruler = new RulerRenderer();`  
` private SequencePanel seqPanel = new SequencePanel();`  
` private Sequence seq;`

` public MultiView() {`  
`   try {`  
`     seq = ProteinTools.createProteinSequence(`  
`         "agcgstyravlivtymaragrsecharlvahklchg",`  
`         "protein 1");`  
`     init();`  
`   }`  
`   catch(Exception e) {`  
`     e.printStackTrace();`  
`   }`  
` }`  
` public static void main(String[] args) {`  
`   MultiView multiView = new MultiView();`  
`   multiView.pack();`  
`   multiView.show();`  
` }`  
` `  
` /**`  
`  * Redefinir pour permettre de terminer le programme.`  
`  */`  
` protected void processWindowEvent(WindowEvent we){`  
`   if (we.getID() == WindowEvent.WINDOW_CLOSING) {`  
`     System.exit(0);`  
`   }`  
`   else {`  
`     super.processWindowEvent(we);`  
`   }`  
` }`  
` `  
` /**`  
`  * Installer les composantes de l'interface`  
`  */`  
` private void init() throws Exception {`  
`   this.setTitle("MultiView");`  
`   this.getContentPane().add(jPanel, BorderLayout.CENTER);`  
`   jPanel.add(seqPanel, BorderLayout.CENTER);`  
`   //ajouter le SymbolSequenceRenderer et le RulerRenderer au MultiLineRenderer`  
`   mlr.addRenderer(symR);`  
`   mlr.addRenderer(ruler);`  
`   //déclarer le MultiLineRenderer comme renderer principal`  
`   seqPanel.setRenderer(mlr);`  
`   //déclarer la  Sequence`  
`   seqPanel.setSequence(seq);`  
`   //déclarer les positions à afficher `  
`   seqPanel.setRange(new RangeLocation(1,seq.length()));`  
` }`

} </java>

[frame|center|Affichage du système de coordonnées d'une
séquence](image:Multiview.jpg "wikilink")
