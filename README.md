# jk_tcpdf
If you need make markers with another color, you're on right page!
* Coloring is at 20px both width and height of the QR.

Just find in your tcpdf folder file tcpdf.php
Then find line:
public function write2DBarcode($code, $type, $x=null, $y=null, $w=null, $h=null, $style=array(), $align='', $distort=false) {

Insert somewhere between standart color style detection:
$mark_color = true;
if (!isset($style['mkcolor'])) {
		$mark_color = false;
}

then in this function find line with:



And replace with this:

		$cur_fg_color = $style['fgcolor'];
		for ($r = 0; $r < $rows; ++$r) {
			$xr = $xstart;
			// for each column
			for ($c = 0; $c < $cols; ++$c) {
				if ($arrcode['bcode'][$r][$c] == 1) {
					// draw a single barcode cell
					if ($mark_color) {
						if ( ( $c>=1 && $c< 5 && $r>=1 && $r<5 ) || 
							( $c>=$cols-5 && $c< $cols-1 && $r>=1 && $r<5 ) ||
							( $c>=1 && $c< 5 && $r>=$rows-5 && $r<$rows-1 )
							) { $cur_fg_color = $style['mkcolor']; } else { $cur_fg_color = $style['fgcolor']; }
					}
					$this->Rect($xr, $ystart, $cw, $ch, 'F', array(), $cur_fg_color );
				}
				$xr += $cw;
			}
			$ystart += $ch;
		}
